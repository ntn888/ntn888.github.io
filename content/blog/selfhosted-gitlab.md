+++
title = "Gitlab Selfhohsted!"
date = 2022-11-23 00:27:00
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

Carrying on my spree of selfhosted software, I moved to hosting my on instance of the free and opensource ==Gitlab==. I think this will be the most rewarding selfhosted project given it's useful nature.

To peek on my installed instance goto [http://gitlab.myjabber.site/](https://gitlab.myjabber.site/ajit).

The good news is that it only requires a cheap less powerful instance. On the flip side we need 2 of those! A dedicated one is recommended for the CI/CD runner application, for acceptable performance. Or it may hurt the responsiveness.

And it was a convenient time to hustle a deal on VPS with black friday on! As being cost conscious as ever; I headed to [Lowendbox](https://lowendbox.com/) to snap a deal. At the time I checked *Racknerd* had a 2.5G deal for 23.50usd yearly. And then for the Gitlab Runner instance, I opted for the CloudServer's 1G for 10usd/yearly. It takes a lot of time to compile the pipiline on some projects but it'd fine for my usecase...

I initially rushed in for a native binary install on the Debian 11 machine I setup. Then I reset the VM and went for the docker based setup, which is just straightforward. Once this is done, you have to download the binary for the runner onto the secondary VM and set it up to attach to the main instance. You will find instructions to do this on the gitlab's documention website.

For those interested here's a copy of my docker-compose.yml.
```
version: '3.6'
services:
  web:
    image: 'gitlab/gitlab-ee:latest'
    restart: always
    hostname: 'gitlab'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'https://gitlab.myjabber.site'
        gitlab_rails['gitlab_shell_ssh_port'] = 2289
        # Add any other gitlab.rb configuration here, each on its own line
        pages_external_url "http://pages.myjabber.site" # not a subdomain of external_url
        #pages_nginx['redirect_http_to_https'] = true
        gitlab_rails['smtp_enable'] = true
        gitlab_rails['smtp_address'] = "smtp.mailgun.org"
        gitlab_rails['smtp_port'] = 587
        gitlab_rails['smtp_authentication'] = "plain"
        gitlab_rails['smtp_enable_starttls_auto'] = true
        gitlab_rails['smtp_user_name'] = "<from address>"
        gitlab_rails['smtp_password'] = "<password_here>"
        gitlab_rails['smtp_domain'] = "<mailgun domain>"
    ports:
      - '80:80'
      - '443:443'
      - '2289:22'
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
    shm_size: '256m'
```

and the .env file in the same folder:
```
GITLAB_HOME=/srv/gitlab
```

~~You may have noticed the use of smtp settings. This is used to mail the users with notifications. I've used Mailgun as my email forwarder; which I think is the goto these days. You can send 2000 mails in the free tier.~~ The free tier has been stopped! Check [this](https://simplycreate.online/update/2022/12/26/postfix_mail.html) for local email forwarder.

Note: I had trouble setting up Gitlab Pages to work on my instance, this may have been because of SSL certificates improperly installed. But I gave-up on this issue and commented out the lines above relating to Pages. I suppose it's not that important on a self hosted instance!?

Finally don't forget to setup the firewall!!


* A note on backup: Given the sensitive nature of one's own code repository it is a no brainer to setup backups. You could use the Oracle's free tier compute VPS for this purpose. We will spinup the AMD [] instead of the ARM Ampere since we dont need resources for this purpose.

Proceed to install rsync on both machines and follow with the script provided [here](https://blog.ssdnodes.com/blog/vps-backups-simple-overthinking/).

