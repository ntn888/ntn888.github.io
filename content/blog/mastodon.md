+++
title = "Self hosted Mastodon Instance"
date = 2022-11-13 00:27:00
draft = false

[taxonomies]
categories = ["update"]
tags = ["selfhosted", "social-media", "servers"]

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

In keeping with the times, I messed about with setting up an own Mastodon self-hosted instance!


~~An ideal VM is the Oracle's OCI cloud service. In there they provide a 4vcore, 24GB ram(!) as a free service. Which is plenty for this purpose. In contrast to the other competitors offering which I found is actually insufficient for self hosting Mastodon. Additionally you get 150G block storage free (in addition to 50G boot volume). Oracle calls it "VM.Standard.A1.Flex".~~

I instead went ahead with LUXVPS. They allow open port on 25 (upon request) for sending emails from local machine without a middleman subscription (and they get expensive quick). They're prices are also very affordable; for 3EUR you get a 1c / 4GB VM with KVM virtualisation. In my experiment this is sufficient... My instance with two test users, with federation relay and full text search only consumes 1G Ram and plenty of processor left. Should be enough for a small gathering :).

---

If you're taking the local server option for emails, you have a good guide here: [https://doc.dovecot.org/configuration_manual/howto/postfix_and_dovecot_sasl/](https://doc.dovecot.org/configuration_manual/howto/postfix_and_dovecot_sasl/). Setting up your own local mail server is tricky with a combination of packages required. This is due to the fact most mail providers (especially Google's GMail) require SASL authentication. But it's worth the effort I think. Again one thing you cannot avoid is that your setup will send the mail almost invariably to the users SPAM folder! But you can always opt for a mail forwarder subscription.

Following on from the above link, some lines has to be commented in /etc/postfix/master.cf:
```
submission inet n - n - - smtpd
  -o smtpd_tls_security_level=encrypt
  -o smtpd_sasl_auth_enable=yes
  -o smtpd_sasl_type=dovecot
  -o smtpd_sasl_path=private/auth
  -o smtpd_sasl_security_options=noanonymous
  -o smtpd_sasl_local_domain=$myhostname
#  -o smtpd_client_restrictions=permit_sasl_authenticated,reject
#  -o smtpd_sender_login_maps=hash:/etc/postfix/virtual
  -o smtpd_sender_restrictions=reject_sender_login_mismatch
#  -o smtpd_recipient_restrictions=reject_non_fqdn_recipient,reject_unknown_recipient_domain,permit_sasl_authenticated,reject
```
Here's the mastodon config (/home/mastodon/live/.env.production)relating to smtp:
```
SMTP_SERVER=localhost
SMTP_PORT=587
SMTP_LOGIN=
SMTP_PASSWORD=
SMTP_FROM_ADDRESS=notifications@yourdomain.online
SMTP_AUTH_METHOD=plain
SMTP_OPENSSL_VERIFY_MODE=none
SMTP_ENABLE_STARTTLS_AUTO=true
```

---


Once you get a VM instance up and running, continue with the official help page: [https://docs.joinmastodon.org/admin/install/](https://docs.joinmastodon.org/admin/install/). I installed on a Debian 11 image.


An 'errata' to the last bit with certbot has input here if you run into problems: [https://github.com/mastodon/documentation/issues/940](https://github.com/mastodon/documentation/issues/940).
Don't forget the prerequisites: [https://docs.joinmastodon.org/admin/prerequisites/](https://docs.joinmastodon.org/admin/prerequisites/)

My instance setup is accessible via [https://simplysocial.online/](https://simplysocial.online/)[^1]. Users can create accounts on this instance!

Stay in touch for further discussion on integrating cloudflare with Backblaze for free egress, in bringing costs down further!

[^1]: In Ubuntu 22.04 you need to run `sudo usermod -a -G mastodon www-data` to fix permissions issue: https://github.com/mastodon/mastodon/discussions/19651
