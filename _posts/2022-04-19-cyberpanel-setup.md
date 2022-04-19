---
title: "Setting up CyberPanel on Ubuntu 20"
date: 2022-04-19T00:30:00+08:00
last_modified_at: 2022-04-19T21:30:00+08:00
categories:
  - Blog
tags:
  - Web
---

My reminder for installing and setting up CyberPanel on Ubuntu 20.

1) Add new user and allow sudo:

```bash
adduser username
usermod -aG sudo username
```

2) Upload ssh keys from external computer:

```bash
ssh-copy-id
```

3) Restrict access to `.authorized_keys` file:

```bash
sudo chmod 700 -R ~/.ssh
sudo chmod 600 ~/.ssh/authorized_keys
```

4) Update Ubuntu packages:

```bash
sudo apt update && sudo apt upgrade
reboot
```


5) Restrict ssh access by editing `sshd_config`.

```bash
sudo vi /etc/ssh/sshd_config
```

- Change the following lines to
```white
PermitRootLogin no
PasswordAuthentication  no
```

- Now restart ssh service.
```
sudo service ssh restart
```

6) Install CyberPanel

```bash
sh <(curl https://cyberpanel.net/install.sh || wget -O - https://cyberpanel.net/install.sh)
```

7) Create new website and add IP address to DNS host.

<!--{% include figure image_path="/assets/images/cyberpanel/create_website.jpg" alt="create website in cyberpanel" caption="" %}-->
![create_website](/assets/images/cyberpanel/create_website.jpg){: .align-center width="250" }

8) Get SSL certification (e.g. Let's Encrypt or Cloudflare)

9) Access from `https://domain.tld:8090`. 

<div class="notice" markdown="1">
**For Cloudflare:** This does not work behind Cloudflare proxy. 

To fix, disable proxy or change CyberPanel port to 8443:

<!--{% include figure image_path="/assets/images/cyberpanel/change_port.jpg" alt="change cyberpanel port" caption="" %}-->
![create_website](/assets/images/cyberpanel/change_port.jpg){: .align-center width="250"}

</div>






