---
title: "Configuring Vaultwarden docker on CyberPanel"
date: 2022-04-25T23:30:00+08:00
last_modified_at: 2023-08-20T01:00:00+08:00
categories:
  - Blog
tags:
  - VPS
  - Docker
toc: true
toc_sticky: true
toc_label: "Vaultwarden"
usemathjax: false
copybutton: true
---

## 1. Pull docker images/cyberpanel/vaultwarden_docker


![details](/assets/images/cyberpanel/vaultwarden_pull.png){: .align-center width="100%"}


## 2. Container details:

![details](/assets/images/cyberpanel/vaultwarden_docker.png){: .align-center width="100%"}

## 3. Create token 

```
openssl rand -base64 48
```

## 4. Add environment variables

```
ADMIN_TOKEN=random_token
WEBSOCKET_ENABLED=true
SIGNUPS_ALLOWED=false
```

## 5. Forward ports in OpenLiteSpeed.

- OLS &rarr; `Virtual Hosts` &rarr; `Context`

- Type `Static`

- Enable HSTS by setting Header Operations:

	```
	Header always set Strict-Transport-Security "max-age=31536000" 
	```
	
	![details](/assets/images/cyberpanel/ols_static_context.png){: .align-center width="100%"}

- Rewrite Rules to redirect HTTP to HTTPS, and to the application:

	<div class="code-header">
		  <button class="copy-code-button btn btn--inverse btn--small">Copy</button>
	</div>
	```
	RewriteEngine On
	RewriteCond %{HTTPS}  !=on
	RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
	RewriteRule (.*)$ http://vaultwarden/$1 [P,L,E=PROXY-HOST:www.website.com]
	```

	![details](/assets/images/cyberpanel/vaultwarden_context_rewrite.png){: .align-center width="100%"}


## 6. Docker notes:


Docker allows external access to opened ports by default. However, this is not required since we're using a reverse proxy to direct traffic to the docker containers.

1. First method:

	<div class="notice" markdown="1">
	To prevent all traffic from external hosts to all containers, we use can use the following iptables rule ([ref](https://serverfault.com/a/1095754)):

	```
	iptables -I DOCKER-USER -i eth0 -m conntrack --ctdir ORIGINAL -j DROP
	```

	To get this to start automatically, add the following in `/etc/rc.local`:

	```
	iptables -N DOCKER-USER
	iptables -I DOCKER-USER -i eth0 -m conntrack --ctdir ORIGINAL -j DROP
	```

	To specify a particular exposed port to be blocked ([ref](https://serverfault.com/questions/704643/steps-for-limiting-outside-connections-to-docker-container-with-iptables/933803#933803)),
	
	```
	iptables -I DOCKER-USER -i eth0 -m conntrack --ctdir ORIGINAL --ctorigdstport 9000 -j DROP
	iptables -I DOCKER-USER -i eth0 -m conntrack --ctdir ORIGINAL --ctorigdstport 3012 -j DROP
	```
	
	This will block external access to ports 9000 and 3012.
	</div>

2.  Another way: 

	<div class="notice" markdown="1">
	Edit or create `/etc/docker/daemon.json` and add:
	```
	{
	   "iptables": false
	}
	```

	Add the following to `/etc/csf/csfpost.sh` (if CSF is installed), or in `/etc/rc.local` [(ref)](https://jsherz.com/docker/configserver/firewall/iptables/csf/debian/systemd/2016/05/16/configuring-configserver-firewall-for-docker.html):
	
	<div class="code-header">
		  <button class="copy-code-button btn btn--inverse btn--small">Copy</button>
	</div>
	```
	#!/bin/sh

	echo "[DOCKER] Setting up FW rules."

	iptables -N DOCKER

	# Masquerade outbound connections from containers
	iptables -t nat -A POSTROUTING -s 172.17.0.0/16 ! -o docker0 -j MASQUERADE

	# Accept established connections to the docker containers
	iptables -t filter -A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT

	# Allow docker containers to communicate with themselves & outside world
	iptables -t filter -A FORWARD -i docker0 ! -o docker0 -j ACCEPT
	iptables -t filter -A FORWARD -i docker0 -o docker0 -j ACCEPT

	echo "[DOCKER] Done."
	```

	Make the file executable:
	
	```
	chmod +x /etc/csf/csfpost.sh
	```

	If CSF is installed, also add the internal IP (e.g. 172.17.0.0/16) to `/etc/csf/csf.allow`.

	</div>

## 7. Use subdirectory

To use a subdirectory e.g. domain.com/bitwarden,

1. Add `External App` for main app:
	- Name: `bitwarden`
	- Address: `localhost:9000`
	
2. Add `External App` for notifications:
	- Name: `bitwardenws`
	- Address: `localhost:3012`

3. Add `Static` Context:
	- URI: `/bitwarden/`
	- Rewrite Base: `/bitwarden/`
	- Rewrite Rules: `RewriteRule (.*)$ http://bitwarden/bitwarden/$1 [P,L,E=PROXY-HOST:domain.com]`
	
4. Add `Static` Context:
	- URI: `/bitwarden/notifications/hub/negotiate/`
	- Rewrite Rules: `RewriteRule (.*)$ http://bitwarden/bitwarden/$1 [P,L,E=PROXY-HOST:domain.com]`
	
5. Add `Static` Context:
	- URI: `/bitwarden/notifications/hub/`
	- Rewrite Rules: `RewriteRule (.*)$ http://bitwardenws/$1 [P,L,E=PROXY-HOST:domain.com]`
	
6. Add `Web Socket Proxy`:
	- URI: `/bitwarden/notifications/hub/'
	- Address: `localhost:3012`