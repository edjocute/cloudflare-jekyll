---
title: "Configuring Vaultwarden docker on CyberPanel"
date: 2022-04-25T23:30:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Web
toc: false
usemathjax: false
---

## 1. Container details:

![details](/assets/images/cyberpanel/vaultwarden_docker.png){: .align-center width="100%"}

## 2. Create token 
```$ openssl rand -base64 48```

## 3. Add environment variables

```gray
ADMIN_TOKEN=random_token
WEBSOCKET_ENABLED=true
SIGNUPS_ALLOWED=false
```

## 4. Edit OpenLiteSpeed settings to forward ports accordingly.

See [here](/blog/jupyter-openlitespeed/) for example.

## 5. Docker notes:


Docker automatically opens ports using iptables.
To prevent this from happening, edit or create `/etc/docker/daemon.json` and add:

```
{
   "iptables": false
}
```
