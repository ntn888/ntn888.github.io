+++
title = "Piracy on a Budget"
date = 2022-11-24 00:27:00
draft = true

[taxonomies]
categories = ["update"]
tags = ["selfhosted", "servers", "seedbox"]

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

The Problem:
- getting a server with resources for Plex serving capabilities is expensive
- getting a server with TB storage is ineffective / moot, then when you have a local server
- running the usual home-based seedbox clogs up your connection by seeding all the time[^1] (you loose your internet's full potential)

The Solution: make a 'smart' remote seedbox (on a cheap VPS) that queue's the torrents according to this remote box's available space and immediately delete the torrent after completing and transferring to the local machine. Then progress to the next pending torrent. The biggest advantage here is that if you do a batch request of torrents (something I do all the time; eg a movie collection) it'll handle progressively without stuffing up the disk space!


Okay that's seems like a lot to digest, but we will do this through rtorrent's scripting capabilities together with remote mount.

![posters](/img/newposters.jpg)

The basic principle is simple. We wil use a remote mount to mount the temporary torrents Download directory to a subdir inside the main media directory. (eg: the *torrents* directory). Then direct radar to this mounted dir as the torrents download directory. Note that we are installing servarr using docker in our local box. If you need instructions on servarr installation see [here](https://wiki.servarr.com/).

The most complex bit is the installing services on the remote box. We need to install vsftp and rtorrent. Luckily there are automated scripts to get this done. I suggest using 'swizzin'. Once this is out of the way; configuring vsftp is the hardest part... We will see this first.

vsftp by default runs on the traditional ftp mode, where a separate 'data' connection is made from the server to the client(!) on port 21. We will setup the passive mode; where we don't need to open firewalls on the client (!). Use the following config /etc/vsftpd.conf:
```
listen=YES
listen_ipv6=NO

user_config_dir=/etc/vsftpd/users
pasv_enable=Yes
pasv_max_port=10100
pasv_min_port=10090
pasv_address=192.9.174.154
local_enable=YES
write_enable=YES
force_dot_files=YES
local_umask=022
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
#connect_from_port_20=NO
utf8_filesystem=YES
require_ssl_reuse=NO
ssl_enable=NO
pam_service_name=vsftpd
secure_chroot_dir=/var/run/vsftpd/empty

#ascii_upload_enable=YES
#ascii_download_enable=YES
#ftpd_banner=Welcome to blah FTP service.

#############################################
#Uncomment these lines to enable FXP support#
#############################################
pasv_promiscuous=YES
port_promiscuous=YES

###################
#Set a custom port#
###################
#listen_port=

```
then in the user config directory named after your system username: /etc/vsftpd/users/ubuntu
```
local_root=/path/to/torrents/dir
dirlist_enable=YES
download_enable=YES
write_enable=YES
```
First do a simple commandline test to test the connection: `pftp my-server-ip`
Next do a manual mount with the command to check: `curlftpfs -o allow_other ftp-user:ftp-pass@my-ftp-location.local /mnt/my_ftp/`. More info on mounting [here](https://linuxconfig.org/mount-remote-ftp-directory-host-locally-into-linux-filesystem)

Now that we have the system prepared we just need to tweak the radarr/sonarr settings to reflect our setup.


Now we come to the meat of this article. Swizzin' goes a long way in glueing up the services; which is rutorrent with the download client rtorrent. The rtorrent setup lives in `~/.rtorrent.rc`.

However this link betweent the client and the rutorrent GUI is done using unix sockets. And works within the local host. But we intend to access rtorrent remotely from our home server. We need to setup a reverse proxy using the already installed nginx server to route this socket to the outside world onto a network port.

To do this insert the following block inside of the preexisting 'http' block:
```
# rtorrent XMLRPC
        server {
           listen 0.0.0.0:8080;
           error_log /var/log/nginx/nginx.error.log;
           access_log /var/log/nginx/access.log;

           auth_basic "Restricted";
           # Users must log on with matching credentials
           auth_basic_user_file /etc/nginx/.htpasswd;

           location /RPC2 {
              include scgi_params;
              scgi_pass unix:/var/run/ubuntu/.rtorrent.sock;
           }
        }
```

Make sure to check the XMLRPC socket path specified in the `.rtorrent.rc` file. Dont forget to open the firewall port!

Next as indicated we need to provide the auth users list file `/etc/nginx/.htpasswd`. This is the credentials we will need to provide in radarr connection settings. To do this apply the following commands[^2]:
`sudo sh -c "echo -n 'sammy:' >> /etc/nginx/.htpasswd"`
`sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"`
and enter a password at the prompts.

Then comeback to radarr settings -> Download Client settings, select rtorrent and fill this:
```
Name: rTorrent
rTorrent host: <your-remote-server-ip>
Port: 8080
URL Path: RPC2
Use SSL: OFF
Username: <your username>
Password: <your password>
Add label to torrent: TV (or anything else you desire)
Optional - Downloaded files location: <custom directory>
```

The final thing remaining to complete our connection is to map the remote folder as explained [here](https://trash-guides.info/Radarr/Radarr-remote-path-mapping/). In our case map the set in the `.rtorrent.rc` as `directory.default.set` to the ftp mount directory. The import will occur between these folders after rtorrent has completed the download.

That's it; we can now test a torrent download from radarr! The only downside to our setup is that we dont see the progress when importing; ie. downloading from the torrent box to our local server.

Now we need to script up rtorrent so that it behaves according to our constraints above.

Actually swizzin' gives us a great plugin that is installed by default that takes care of this by itself: QUOTA.

This will manage the disk space issue automatically; just point Quota to the right volume you intend to use for downloads folder when installing swizzin'.

All we need to do now is set the queue length in rutorrent and setup auto-deletion in the .rtorrent.rc file when the ratio is reached. Add this to the file[^3]:

```
ratio.enable=
ratio.min.set=200
method.set = group.seeding.ratio.command, "d.close= ; d.erase="
```

All Done!

[^1]: I refer to power user's with private trackers that require ratio maintaining.
[^2]: [https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-set-up-password-authentication-with-nginx-on-ubuntu-14-04)
[^3]: [https://github.com/rakshasa/rtorrent/wiki/RTorrentRatioHandling](https://github.com/rakshasa/rtorrent/wiki/RTorrentRatioHandling)
