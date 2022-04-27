---
title: "Configuring OpenLiteSpeed for Jupyterlab"
date: 2022-04-25T06:00:00+08:00
#last_modified_at: 2022-04-20T21:30:00+08:00
categories:
  - Blog
tags:
  - Python
  - Web
toc: false
usemathjax: false
---

We assume that JupyterLab is already installed.

## 1. Configure JupyterLab/Hub

1. Generate `jupyter_notebook_config.py` file:
```bash
$ jupyter notebook --generate-config
```

2. Edit `jupyter_notebook_config.py`

- Change base_url:
```white
c.NotebookApp.base_url = '/jlab'
```

- Change allowed hostnames:
```white
c.NotebookApp.local_hostnames = ['localhost', 'hostname.com']
```

## 2. Start Jupyter automatically

We'll start Jupyterlab on port 8888. Change the directories according to your installation.

- Create script:
```$ vi ~/start-jupyter.sh```

```white
#!/bin/bash

/home/user/miniconda3/envs/jup/bin/jupyter-lab \
        --notebook-dir=/home/user \
        --port=8888 \
        --ContentsManager.allow_hidden=True \
        --no-browser
```

- Add new service:
```$ vi /etc/systemd/system/jupyter.service```

```white
[Unit]
Description=Jupyter Notebook

[Service]
Type=simple
PIDFile=/run/jupyer.pid
ExecStart=/home/user/start-jupyter.sh
User=user
Group=group
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target                 
```

- Start service
```bash
$ sudo service jupyter enable
$ sudo service jupyter start
```

## 3. Configure OpenLiteSpeed (OLS)

No need to use rewrite rules here. Select your site on OpenLiteSpeed panel and add the following:

1. External App
- Type: `Web Server`
- Name: `jupyter`
- Address: `localhost:8888`
- Max Connections: `5`
![external app](/assets/images/cyberpanel/OLS_externalapp.png){: .align-center width="100%"}

2. Context
Use the same base_url as above.
- Type: `Proxy`
- URI: `/jlab`
- Web Server: select `name` from step above
![context](/assets/images/cyberpanel/OLS_context.png){: .align-center width="100%"}


3. Web Socket Proxy.
This is required for the kernel connection.
- URI `/jlab`
- Address: `localhost:8888`

4. Restart OLS