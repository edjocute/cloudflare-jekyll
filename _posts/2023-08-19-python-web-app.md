---
title: "Serving Python Flask App with Gunicorn and Nginx"
date: 2023-08-19T23:30:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - VPS
  - Python
toc: true
usemathjax: false
copybutton: true
---

Say we have created a Python web application, perhaps using Flask or Django3. Here, we'll go through how to serve the webpage using the Gunicorn server running behind a reverse proxy.

Here, I assume that your home directory is `/home/ubuntu/` with the app located at `/home/ubuntu/myapp`

## 1. Install Python (Miniconda)

We'll set up our Python environment using Miniconda.

1. Download Miniconda for Linux from the [Conda website](https://docs.conda.io/en/latest/miniconda.html)
	```sh
wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-Linux-aarch64.sh
	```
	
	Make sure to choose the correct architecture.
	
2. Install Miniconda and follow instructions.
	```sh
bash Miniconda3-py39_4.11.0-Linux-aarch64.sh
	```

3. Make sure the base environment is loaded
	```sh
source ~/.bashrc
	```
	
4. We'll use `mamba` instead of `conda` to install packages since it resolves packages faster.
	```sh
conda install mamba -n base -c conda-forge
	```
	
5. Now create a new environment with your chosen Python version and activate the new environment.
	```sh
conda create -n myenv python=3.9
conda activate myenv
	```
	
6. Install Flask, Gunicorn and other required packages.
	```sh
mamba install flask flask-bootstrap flask-nav gunicorn 
	```

That concludes the required installation.	

## 2. Create wsgi.py file

`wsgi.py` should be similar to your Flask `app.py`, e.g.:
{% include codeHeader.html %}
```python
from flaskapp import app

if __name__ == '__main__':
    app.run()
```

## 3. Create Systemd service to run the web application automatically

Create file `/etc/systemd/system/myapp.service` using the appropriate directories:
{% include codeHeader.html %}
```systemd
[Unit]
Description=My Web Application
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/myapp
Environment="PATH=/home/ubuntu/miniconda3/bin"
ExecStart=/home/ubuntu/miniconda3/envs/myenv/bin/gunicorn -w 2 --bind unix:myapp.sock wsgi:app
Restart=always

[Install]
WantedBy=multi-user.target
```

Start the service and ensure that it runs successfully.
```sh
sudo systemctl enable myapp
sudo systemctl start myapp
sudo systemctl status myapp
```

## 4. Configure reverse proxy

### Nginx

We'll now use Nginx as reverse proxy so that the application is accessible at your url. 
For security reasons, the site will be accessible in https while http requests will be redirected.
This requires SSL certificates e.g. by Let's Encrypt.
Cloudflare cerficates can also be used if the site will be proxied through Cloudflare proxy.

Create the file `/etc/nginx/sites-available/myapp`
{% include codeHeader.html %}
```nginx
server {
    listen 80;
    listen [::]:80;
    server_name myapp.domain.com;

    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name myapp.domain.com;

    ssl_certificate /etc/nginx/ssl/domain.com-cert.pem;
    ssl_certificate_key /etc/nginx/ssl/domain.com-key.pem;

    client_max_body_size 5M;

    location /static {
        include /etc/nginx/mime.types;
        #root /home/ubuntu/myapp/;
        alias /home/ubuntu/myapp/static;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/ubuntu/myapp/myapp.sock;
    }
}
```
Note that if you have any static files, it is more efficient for them to be served directly using Nginx. 
Here, this only applies to files included in `/etc/nginx/mime.types`.


Now create a symlink of this file in the `sites-enabled` folder:
```sh
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/`
```

Finally, restart Nginx to load the new config and ensure that there are no errors:
```sh
sudo systemctl restart nginx
```

### 5. Done!

At this stage, your Python app should be accessible from domain.com

