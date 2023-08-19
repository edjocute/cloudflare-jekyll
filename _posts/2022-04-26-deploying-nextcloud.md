---
title: "Deploying Nextcloud"
date: 2022-04-26T22:30:00+08:00
last_modified_at: 2023-08-20T01:00:00+08:00
categories:
  - Blog
tags:
  - Web
toc: true
usemathjax: false
copybutton: true
---

Docker-compose can be easily used to deploy Nextcloud on your own webserver using the official images available at Docker Hub.
Apart from the main Nextcloud app, we go through how to set up the opensource online office suite Collabora which can serve as a replacement for Google Docs.

This set of notes is a compilation of instructions I have found online (forums, Reddit, Stackover etc.) and have worked for me on Ubuntu with Caddy for Nextcloud version 25.


## Docker-compose

In addition to the main app, we also need to set up and configure efficient databases for Nextcloud e.g. MariaDB and Redis. It's easy to use docker-compose and include all the required services

### 1. Create nextcloud directory
Begin by choosing a folder to store the Compose file `docker-compose.yml`. If your docker directory is `/opt/docker`, then create the folder `/opt/docker/nextcloud/`.


### 2. Create `docker-compose.yml`
Copy the following into `docker-compose.yml`.

{% include codeHeader.html %}
```yml
version: '3.7'

volumes:
  nextcloud:
  db:

networks:
  nextcloud:
    driver: bridge
    driver_opts:
      com.docker.network.bridge.name: br-nextcloud
    ipam:
      driver: default
      config:
        - subnet: ${IPV4_NETWORK}.0/16


services:

  # Nextcloud
  app:
    image: nextcloud:stable
    container_name: nextcloud
    networks:
		nextcloud:
			ipv4_address: ${IPV4_NETWORK}.2
    restart: unless-stopped
    ports:
      - 127.0.0.1:9001:80
    links:
      - db
      - redis
    volumes:
      - ${NEXTCLOUD_ROOT}/html:/var/www/html
      - ${NEXTCLOUD_ROOT}/data:/media/ncdata
    environment:
      - NEXTCLOUD_DATA_DIR=/media/ncdata
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
      - REDIS_HOST=redis
      - REDIS_HOST_PASSWORD=${REDIS_HOST_PASSWORD}
      #- TRUSTED_PROXIES=${IPV4_NETWORK}.1
      - NEXTCLOUD_TRUSTED_DOMAINS=${NEXTCLOUD_FQDN}
      - OVERWRITEPROTOCOL=https
      - PHP_MEMORY_LIMIT=2048M
      - PHP_UPLOAD_LIMIT=2048M
    depends_on:
      - db
      - redis
	  
	  
  # MariaDB Database
  db:
    image: mariadb:latest
    container_name: nextcloud-mariadb
    networks:
		nextcloud:
			ipv4_address: ${IPV4_NETWORK}.3
    restart: unless-stopped
    command: ['--transaction-isolation=READ-COMMITTED', '--binlog-format=ROW', '--innodb_read_only_compressed=OFF']
    volumes:
      - ${NEXTCLOUD_ROOT}/mariadb:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud


  # Redis
  redis:
    image: redis:latest
    container_name: nextcloud-redis
    networks:
		nextcloud:
			ipv4_address: ${IPV4_NETWORK}.4
    restart: unless-stopped
    command: redis-server --requirepass ${REDIS_HOST_PASSWORD}


  # Collabora
  collabora:
    image: collabora/code
    container_name: collabora
    restart: unless-stopped
    networks:
		nextcloud:
			ipv4_address: ${IPV4_NETWORK}.7
    ports:
      - 127.0.0.1:9980:9980
        #extra_hosts:
        #- "${NEXTCLOUD_FQDN}:${NEXTCLOUD_IPADDRESS}"
        #- "${COLLABORA_FQDN}:${NEXTCLOUD_IPADDRESS}"
    #volumes:
    #  - ${NEXTCLOUD_ROOT}/collabora/coolwsd.xml:/etc/coolwsd/coolwsd.xml
    environment:
      - domain=${NEXTCLOUD_FQDN}
      #- server_name=${COLLABORA_FQDN}
      #- aliasgroup1=
      - dictionaries=en
      - username=admin
      - password=adminpassword
      - extra_params=--o:ssl.enable=true
    cap_add:
      - MKNOD
    tty: true

  # Push Notifications
  notify_push:
    image: nextcloud:stable
    container_name: nextcloud-notify_push
    restart: unless-stopped
    networks:
		nextcloud:
			ipv4_address: ${IPV4_NETWORK}.5
    ports:
      - 127.0.0.1:7867:7867
    links:
      - db
      - redis
    depends_on:
      - db
      - redis
      - app
    volumes:
      - ${NEXTCLOUD_ROOT}/html:/var/www/html:ro
    environment:
      - PORT=7867
      - NEXTCLOUD_URL=http://nextcloud/
      - DATABASE_URL=mysql://nextcloud:${MYSQL_PASSWORD}@db/nextcloud
      - DATABASE_PREFIX=oc_
      - REDIS_URL=redis://:${REDIS_HOST_PASSWORD}@redis
    entrypoint: /var/www/html/custom_apps/notify_push/bin/x86_64/notify_push /var/www/html/config/config.php
	
	
  # Coturn server
  coturn:
    image: coturn/coturn
    container_name: nextcloud-coturn
    restart: unless-stopped
    #network_mode: host
    networks:
		nextcloud:
			ipv4_address: ${IPV4_NETWORK}.5
    ports:
      - 127.0.0.1:2052:2052/tcp
      - 127.0.0.1:2052:2052/udp
        #- 5349:5349/tcp
        #- 5349:5349/udp
        #volumes:
            #- /etc/letsencrypt/live/cloud.darkhalo.science/:/certs:ro
    command:
      - -n
      - --log-file=stdout
      - --realm=${NEXTCLOUD_FQDN}
      - --listening-port=2052
      #- --tls-listening-port=5349
      - --listening-ip=0.0.0.0
      #- --relay-ip=${NEXTCLOUD_IPADDRESS}
      - --external-ip=${NEXTCLOUD_IPADDRESS}
      - --no-tlsv1
      - --no-tlsv1_1
      #- --cert=/certs/cert.pem
      #- --pkey=/certs/privkey.pem
      - --min-port=49160
      - --max-port=49200
      #- --user=test:test123
      #- --no-multicast-peers
      #- --lt-cred-mech
      - --use-auth-secret
      - --static-auth-secret=${COTURN_SECRET}
```

- Here, we have provided a subnet so the stack will be accessible at a known address, 172.28.0.x in this case.
- By specifying the ports as `127.0.0.0:xxxx:xxxx`, the services will only be accessible to localhost or via a reverse proxy (next section). This prevents the ports from being exposed to external networks. When troubleshooting, it might be useful to remove `127.0.0.1` and test that the services can be accessed remotely from the external server ip address.
- In this example, we have the main Nextcloud service running on port `9001`.
- For Collabora, the `domain` variable should be the domain name of the nextcloud server, not the collabora server. [(ref)](https://help.nextcloud.com/t/no-acceptable-wopi-hosts-found-matching-the-target-host/12392)

### 3. Create environment file
The above `docker-compose.yml` relies on some variables which need to be defined. 
These go into a `.env` file.

{% include codeHeader.html %}
```
NEXTCLOUD_ROOT=/opt/docker/nextcloud
NEXTCLOUD_IPADDRESS=x.x.x.x
NEXTCLOUD_FQDN=cloud.domain.com
COLLABORA_FQDN=office.domain.com
IPV4_NETWORK=172.28.1

MYSQL_ROOT_PASSWORD=xxx
MYSQL_PASSWORD=xxx
REDIS_HOST_PASSWORD=xxx
COTURN_SECRET=xxx
```
- Chosse the NEXTCLOUD_ROOT directory accordingly.
- NEXTCLOUD_IPADDRESS is the ip address of the server.
- The two FQDNs are the fully qualified domain names where Nextcloud and Collabora will be available at.
- To generate random hex keys,
 ```sh
openssl rand -hex 32
 ```

- For hash keys,
 ```sh
openssl rand -base64 16
```
- Secure the passwords and secrets by making sure `.env` is only accessible to root:
 ```sh
sudo chmod 0700 .env
```
- Check that the environmental variables work:
 ```sh
sudo docker-compose config
```

### 4. Start the container stack

In the `/opt/docker/nextcloud` directory, run
```sh
sudo docker-compose up -d
```
The first run will take a moment for the databases to be created and set up.



## Configuring Reverse Proxy

For the server to be accessible from a web url, we configure a reverse proxy to direct the external requests to the appropriate internal ports without having the ports exposed externally.

Here, use the lightweight web server Caddy 2, and OpenLiteSpeed. 

### Caddy

The caddy configuration file is by default located at `/etc/caddy/Caddyfile`. Add the following sections for Nextcloud and Collabora.

{% include codeHeader.html %}
```
# Nextcloud
@nextcloud host cloud.domain.com
handle @nextcloud {
   header Strict-Transport-Security max-age=15552000;

   encode zstd gzip

   redir /.well-known/carddav /remote.php/dav 301
   redir /.well-known/caldav /remote.php/dav 301

   handle_path /push/* {
      reverse_proxy http://127.0.0.1:7867
   }

   reverse_proxy http://127.0.0.1:9001

   @forbidden {
        path    /.htaccess
        path    /data/*
        path    /config/*
        path    /db_structure
        path    /.xml
        path    /README
        path    /3rdparty/*
        path    /lib/*
        path    /templates/*
        path    /occ
        path    /console.php
   }
   respond @forbidden 404
}

# Collabora
@collabora host office.domain.com
handle @collabora {
	encode gzip zstd
   
	reverse_proxy https://localhost:9980 {
		header_up Host {host}
        header_up Connection "Upgrade"
        header_up Upgrade websocket
        transport http {
			tls_insecure_skip_verify
        }
	}

}
```

- For the calendar and contact sync to function properly, we need to redirect and make sure `/.well-known/carddav` and `/.well-known/caldav` are accessible.
- For push service, `/push/` has to be redirected to the coturn service.
- There have been bugs allowing users to access files in the Nextcloud directory from the url. To ensure this does not happen, we specify files and folders to which access is forbidden.
- The configuration for Collabora is courtesy of [(this forum post)](https://caddy.community/t/help-with-getting-caddy-working-with-nextcloud-and-collabra/14543).


---

## Other tips and configuration

At this point, hopefuilly your Nextcloud server is up and running and accessible from the outside world. 
Log in as admin and enter admin settings. Check if any warnings displayed. 

### 5. Fix security warnings


- Add the following to `config/config.php` [(ref)](https://github.com/nextcloud/docker/issues/800):

	```php
  'trusted_proxies' =>
  array (
	0 => '172.28.0.1',
  ),
  'forwarded-for-headers' =>
  array (
	0 => 'HTTP_X_FORWARDED_FOR',
  ),
	```


- Alternatively, use the command:
 ```sh
docker exec --user www-data nextcloud php occ config:system:set trusted_proxies 1 --value='172.28.0.1'
```

- Check the correct IP value using:
 ```sh
docker network inspect nextcloud_nextcloud
```

### 6. Setup 2FA

- Follow instructions [here](https://nextcloud-twofactor-gateway.readthedocs.io/en/latest/Admin%20Documentation/).

- Execute command:
 ```sh
docker exec -u www-data -it nextcloud php occ twofactorauth:gateway:configure telegram
```

### 7. Cron job ([ref](https://blog.mariu5.de/~/MariusBlog/the-easiest-way-to-set-up-cron-for-the-nextcloud-docker-image/))

- In the host:
 ```sh
crontab -e
```

- Add the lines:
 ```
*/5 * * * * docker exec -u www-data nextcloud php cron.php
```


### 8. High Performance Backend

1. Install app
	- `docker-compose up` for `notify_push` will fail the first time since the required app is not installed. 
	- Once initial setup for Nextcloud is complete, install `Client Push`  app.
	- Run `docker-compose up -d` again, which should complete without errors.

2. Setup app
	- Run the command:
    ```sh
docker exec -u www-data nextcloud php occ notify_push:setup https://cloud.domain.com/push
	```
		
	- Everything should pass if all goes well.
	
	- If `push server is not a trusted proxy` is encountered, let notify_push connect locally to the Nextcloud instance.
		1. Add `nextcloud` to `trusted_domains'
		2. Set `NEXTCLOUD_URL=http://nextcloud/` in `docker-compose.yml`

3. Verification
	- Browser console should not have errors connecting to websockets.
	
	- Using logs:
 ```sh
docker logs nextcloud
```

	- Using metrics:
 ```sh
docker exec -u www-data nextcloud php occ notify_push:metrics
```
	
	If successful, connection counts should be more than zero:
	![push](/assets/images/cyberpanel/notify_push.png){: .align-center width="40%"}
	


### 9. Imagemagick

{% include codeHeader.html %}
```console
docker exec nextcloud bash -c 'apt update && apt-get install -y --no-install-recommends $(apt-cache search libmagickcore-6.q[0-9][0-9]-[0-9]-extra | cut -d " " -f1)'
```

### 10. Phone region

{% include codeHeader.html %}
```shell
docker exec --user www-data nextcloud php occ config:system:set default_phone_region --value=US
```

### 11. Using Nextcloud in URL path

To use a path in the url e.g. `domain.com/nextcloud/`, set `overwritewebroot` and `overwrite.cli.url` in `config/config.php` accordingly:

```shell
docker exec -u www-data nextcloud php occ config:system:set overwritewebroot --value=/nextcloud
docker exec -u www-data nextcloud php occ config:system:set overwrite.cli.url --value=https://domain.com/nextcloud
```


<!--
### 12. Collabora

Since we're behind a reverse proxy handling SSL, we need to disable the internal SSL for Collabora.

- Edit `coolwsd.xml`:
	-  Copy `coolwsd.xml` from the container:
	
		```console
		docker cp collabora:/etc/coolwsd/coolwsd.xml ./collabora/coolwsd.xml
		```

	- Under `<ssl desc="SSL settings">`, the two lines should say the following:
	
		```
		<enable type="bool" desc="Controls whether SSL encryption between coolwsd and the network is enabled (do not disable for production deployment). If default is false, must first be compiled with SSL support to enable." default="true">false</enable>
		<termination desc="Connection via proxy where coolwsd acts as working via https, but actually uses http." type="bool" default="true">true</termination>
		```

	- Mount the file as shown in `docker-compose.yml`
-->


### 12. Resetting configuration

To clear Nextcloud configuration and redo the setup, there are two options:

1. Navigate to Nextcloud folder e.g. `/home/site.com/public_html/nextcloud/`.
	- Delete the folder `data`
	- Delete the file `config/config.php`
	- Create a file `config/CAN_INSTALL`


2. If using `docker-compose`, then simply remove the volumes e.g.
 ```sh
docker volume rm nextcloud_db
docker volume rm nextcloud_nextcloud
```
	
### 13. Updating Nextcloud

1. Nextcloud can be simply updated and restarted by

	```sh
sudo docker-compose pull
sudo docker-compose down
sudo docker-compose up -d`
```
	
2. However, Nextcloud can only be updated one major version at a time. In this case, specify the specific version in `docker-compose.yml` e.g. 

	```yml
image: nextcloud:25
```

3. After updating, verifying the security and setup warnings. If the warning `The database is missing some indexes. Due to the fact that adding indexes on big tables could take some time they were not added automatically.` is obtained, run the following command ([ref](https://www.reddit.com/r/NextCloud/comments/r9zw8r/how_to_run_occ_command_to_add_missing_indices/)):

	```sh
sudo docker exec --user www-data nextcloud_app php occ db:add-missing-indices
```
	
4. If MariaDB is updated, we can update the database as such:

	```sh
source .env && docker-compose exec nextcloud-mariadb mysql_upgrade -uroot -p${MARIADB_ROOT_PASSWORD}
```


---

## Docker

Docker can also be used manually without docker-compose. This can be useful if the web server provides a way to manage docker services e.g. CyberPanel.

### 1. mariadb ([ref](https://hevodata.com/learn/mariadb-docker/))

mariadb (MySQL replacement) is installed with Cyber Panel. 
However, the mysqld server binds by default to 127.0.0.1, which cannot be accessed from within a Docker container.

To enable the Nextcloud container to access mariadb, setup a separate mariadb container instance.


- Use port `3307` since `3306` is in use by host already.

- Add root password as environment variable:

	```
MYSQL_ROOT_PASSWORD:***
```

- Create and start container.


### 2. Nextcloud 


- Create nextcloud folder:

	```sh
mkdir -p /home/cloud.site.com/public_html/nextcloud
```

- Create Nextcloud docker from CyberPanel.

- Map volume
`/var/www/html` to `/home/cloud.site.com/public_html/nextcloud`

- Create and start container.



### 4. Initial set up

- Select `MySQL/MariaDB` as the database and enter details:
![details](/assets/images/cyberpanel/nextcloud_create.png){: .align-center width="35%"}

## CyberPanel/OpenLiteSpeed

### Nextcloud
1. In OpenLiteSpeed &rarr; `Virtual Host` &rarr; site &rarr; Create `External App`:
	- Name: nextcloud
	- URI: localhost:9001

2. In OpenLiteSpeed, create reverse proxy using rewrite rules [(ref)](https://community.cyberpanel.net/t/how-to-write-rewrite-rules-for-openlitespeed/30658):
	- `Virtual Hosts` &rarr; `Context`
	- Type: `Static`
	- URI: `/`
	- Enable HSTS by setting Header Operations:
	
		```
Header always set Strict-Transport-Security "max-age=31536000" 
		```
		
		![details](/assets/images/cyberpanel/ols_static_context.png){: .align-center width="80%"}

	- Rewrite Rules to redirect (i) HTTP to HTTPS, (ii) HTTPS to application, (iii) other required paths.
		
		<div class="code-header">
		  <button class="copy-code-button btn btn--inverse btn--small">Copy</button>
		</div>
		```bash
RewriteRule ^\.well-known/carddav https://%{SERVER_NAME}/remote.php/dav/ [R=301,L]
RewriteRule ^\.well-known/caldav https://%{SERVER_NAME}/remote.php/dav/ [R=301,L]
RewriteCond %{HTTPS} !=on
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
RewriteRule (.*)$ http://nextcloud/$1 [P,L,E=PROXY-HOST:cloud.domain.com]
		```
		
		![details](/assets/images/cyberpanel/ols_static_rewrite.png){: .align-center width="80%"}


	- Restart OpenLiteSpeed

3. Debugging:

	<div class="notice" markdown="1">
	**Debug Rewrite Rules**: 
	1. in `Virtual Hosts` &rarr; website &rarr; `Rewrite`:
	
		```
Log Level: 9
		``` 

	2. Check error logs:
	```sh
tail -f /usr/local/lsws/logs/error.log
	```
	</div>
	
OpenLiteSpeed does not support `SetEnvIf` for headers (e.g. `env=HTTPS` does not work) ([ref](https://forum.openlitespeed.org/threads/access-control-allow-origin-multiple-origin-domains.4359/)).

### Collabora 
- In Cyberpanel, create a new website e.g. `office.domain.com`

- Set up reverse proxy:
	- In OLS, add an `External App`:
		- Type: `Web Server`
		- Name: `collabora`
		- Address: `localhost:9980`
	
	
	- Add `Context`:
		- Type: `Static`
		- URI: `/`
		- Rewrite Rules:
		
			```
RewriteCond %{HTTPS} !=on
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
RewriteRule (.*)$ http://collabora/$1 [P,L,E=PROXY-HOST:office.domain.com]
			```
		
	- Add `Web Socket Proxy`:
		- URI: `/`
		- Address: `localhost:9980`


### High Performance Backend
- Setup reverse proxy for the newly created virtual host in OpenLiteSpeed:
	- In OLS, add `External App`:
		- Name: `notify_push`
		- Address: `localhost:7867`
		- Max Connections: 25
		- Timeout: 60
		
	- Add `Context`:
		- Type: `static`
		- URI: `/push/`
		- Rewrite Rules: 
		```
RewriteRule (.*)$ http://notify_push/$1 [P,L,E=PROXY-HOST:cloud.domain.com]
		```

	- Add `Web Socket Proxy`:
		- URI: `/push/`
		- Address: `localhost:7867`
		
---

