# Azure Cockpit Networking Servers

## Overview

Home directory structure (`/home/azureuser` or `~`):
- app.py
- mirror-test/ (or Build-Networking-Server/)
- pull.sh


### Flask

The server runs a Flask application:

`~/app.py`

- Provides a webhook endpoint that can be triggered from GitHub
- Executes a bash script to pull from a git repo, in this case the repo contains the build executable
- Provides a web browser accessible status of Docker and git
- Web addresses are
  - `:8000/`
  - `:8000/postreceive`
  - `:8000/logs`

```python
from flask import Flask
import subprocess

app = Flask(__name__)

@app.route("/postreceive", methods=['GET', 'POST'])
def hello_world():
    subprocess.call(['sudo', '/home/azureuser/pull.sh'])
    return "<p>Hello, World!</p>"


@app.route("/", methods=['GET', 'POST'])
def hello_world_home():
    return "<code><h3>docker ps:</h3>"+subprocess.check_output(['docker', 'ps']).decode("utf-8").replace("\n", "<br>")+"<br><br><h3>git status:</h3>"+subprocess.check_output(['git', '--git-dir', '/home/azureuser/mirror-test/.git', 'status']).decode("utf-8").replace("\n", "<br>")+ "</code>"



@app.route("/logs", methods=['GET', 'POST'])
def hello_world_logs():
    ps = subprocess.check_output(['docker', 'ps', '-q']).decode("utf-8").replace("\n","")
    return "<code>"+subprocess.check_output(['docker', 'logs', ps]).decode("utf-8").replace("\n", "<br>")+ "</code>"

```


The Flask application is served by gunicorn running as a service:

`/etc/systemd/system/gunicorn.service`

```ini
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=azureuser
Group=azureuser
WorkingDirectory=/home/azureuser
ExecStart=/home/azureuser/.local/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 'app:app'

[Install]
WantedBy=multi-user.target
```
If any changes are made to app.py:

`sudo service gunicorn restart`

---

### Bash script

`~/pull.sh`

```bash
#!/bin/bash

eval "$(ssh-agent)"
ssh-add /home/azureuser/.ssh/id_rsa
cd /home/azureuser/mirror-test
git pull
c=$(docker ps -q) && [[ $c ]] && docker kill $c
docker build -t mirror-test .
docker run -d -p 7777:7777/udp mirror-test
```

---

### Github WebHook:

![image](https://github.com/Spark-games/knowledge-Base/assets/370601/38d4d881-c0a6-45c9-90aa-4f97892dcb64)


It runs a Docker container that runs the server executable as per https://docs.edgegap.com/docs/tools-and-integrations/unity-server-in-docker/

The github repo that is pulled by the bash script above should contain three parts:
1. `Dockerfile`

```
FROM ubuntu:bionic
MAINTAINER Edgegap <youremail@edgegap.com>

ARG DEBIAN_FRONTEND=noninteractive
ARG DOCKER_VERSION=17.06.0-ce

EXPOSE 7777:7777/udp

RUN apt-get update && \
apt-get install -y libglu1 xvfb libxcursor1

COPY build/                  /root/build/
COPY entrypoint.sh           /entrypoint.sh

WORKDIR /root/
ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
```

2. `entrypoint.sh`

```bash
chmod +x /root/build/Build.x86_64
xvfb-run --auto-servernum --server-args='-screen 0 640x480x24:32' /root/build/Cockpit.x86_64 -batchmode -nographics
```

3. `build/` a folder containing the linux executable headless server, in this case named `Cockpit.x86_64`

---

### Git repo

Run `git clone ...` to clone the build repo, in this case this creates the folder `~/mirror-test/`
