---
title: "Setting up CyberPanel on Ubuntu 20"
date: 2022-04-19T00:30:00+08:00
categories:
  - blog
tags:
  - web
---

These instructions serve to collect the instructions for installing and setting up CyberPanel on Ubuntu 20.

1) Add new user and allow sudo:

```bash
adduser username
usermod -aG sudo username
```

2) Set up ssh access from external computer:

```bash
ssh-copy-id
```

3) 

```bash
sudo chmod 700 -R ~/.ssh
sudo chmod 600 ~/.ssh/authorized_keys
```

4) Update Ubuntu packages:

```bash
sudo apt update && sudo apt upgrade
reboot
```


5) Restrict ssh access:

```bash
sudo vi /etc/ssh/sshd_config
PermitRootLogin no
PasswordAuthentication  no
sudo service ssh restart
```

6) Now install CyberPanel

```bash
sh <(curl https://cyberpanel.net/install.sh || wget -O - https://cyberpanel.net/install.sh)
```

7) Create new website and add IP address to DNS host.

8) Get SSL certification (e.g. Let's Encrypt or Cloudflare)

9) Access from https://name.tld:8090. 
However, does not work behind Cloudflare proxy. To fix, change CyberPanel port to 8443 or disable Cloudflare proxy.






