+++
title = "Selfhosted email delivery with postfix"
date = 2022-12-27
draft = false

[taxonomies]
categories = ["update"]
tags = ["selfhosted", "servers", "open-source"]

[extra]
lang = "en"
toc = true
comment = false
copy = true
math = false
mermaid = false
outdate_alert = false
outdate_alert_days = 120
display_tags = true
truncate_summary = false
+++

Note that it is assumed that a Gitlab docker instance is setup on a Debian 10/11 machine... This article follows the [post](https://simplycreate.online/update/2022/11/23/selfhosted-gitlab.html).

According to the docs, the official image of Gitlab doesn't include a Mail Transfer Agent (MTA) preinstalled. For our purposes this is the program that routes out the email from our system. So we'll install it on the host system and setup Gitlab to route mail to it.

This is simple enough, but getting our local hosted email service to play nice with Gmail is a different story.

We begin by setting up basic postfix email. Install `postfix` as per [this](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-on-ubuntu-20-04). We only need *step one*.

To test:
```
echo "Subject: test" | sendmail -v mymail@mydomain.com
```

This will not actually deliver the mail as yet; but atleast it should run without errors!



To allow the docker container to access postfix we make a small change to `/etc/postfix/main.cf`. Add `0.0.0.0/0` to `mynetworks`. This allows any IP to initiate email, as IP address change with every container restarts, we will take this approach.

Remember to restart postfix on every edit to this file by:
```
sudo systemctl reload postfix
```

Then add this smtp settings in our Gitlab's `docker-compose.yml`:
```
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "gitlab.example.com"
gitlab_rails['smtp_port'] = 25
```

Note that `'smtp_address'` will be the FQDN that we set as our host machine domain.

Now to test, execute the following:
```
sudo docker exec -it  gitlab-compose-web-1 bash
gitlab-rails console -e production

Notify.test_email('mymail@mydomain.com', 'Message Subject', 'Message Body').deliver_now
```

In the first command `gitlab-compose-web-1` is the name of the container. To check your running name run `sudo docker ps`.

Note this will not yet work if you're using a Gmail addresses read on to fix this...


Setting up SASL (for Gmail receipients)
---------------------------------------

Next thing to do is to install Dovecot. On debian:
```
sudo apt install dovecot-core dovecot-pop3d dovecot-imapd
```

Then in `/etc/dovecot/conf.d/10-master.conf`:
```
service auth {
  unix_listener auth-userdb {
    mode = 0666
    user = postfix
    group = postfix
  }

  # Postfix smtp-auth
  #unix_listener /var/spool/postfix/private/auth {
  #  mode = 0666
  #}

  # Auth process is run as this user.
  #user = $default_internal_user
}
```

Don't forget to restart dovecot:
```
sudo systemctl restart dovecot
```

Then modify postfix to use dovecot ADD `/etc/postfix/main.cf`:
```
smtpd_sasl_type = dovecot
```

Don't forget to restart postfix:
```
sudo systemctl reload postfix
```

Lastly we need to add a TXT record for our domain:
```
v=spf1 ip4:x.x.x.x ~all
```
where `x.x.x.x` is the IP of the server.

Now try:
```
echo "Subject: test" | sendmail -v me@gmail.com
```

Tip: handy commands to view the log messages:
postfix: `sudo tail -f /var/log/mail.log`
Dovecot: `sudo tail -f /var/log/syslog`


