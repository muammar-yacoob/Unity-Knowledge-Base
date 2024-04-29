# Setup a private Unity Package Manager with Verdaccio

## Requirements

- A virtual machine running Ubuntu 20.04
- Ports 80 and 443 exposed

## Install node js

Install nvm:

https://github.com/nvm-sh/nvm#installing-and-updating

Install node 18:

`nvm install v18.16.1`

You may need to 'use' the newly installed version:

`nvm use v18.16.1`


## Verdaccio

Install Verdaccio:

https://verdaccio.org/docs/installation


## Github OAuth Plugin

Install Github OAuth Plugin:

https://github.com/n4bb12/verdaccio-github-oauth-ui

https://github.com/n4bb12/verdaccio-github-oauth-ui/blob/master/docs/configuration.md

## Verdaccio config

`nano /home/azureuser/verdaccio/config.yaml`

Add or update the following:


```
publicPath: https://upm.born.net/

packages:
    '@*/*':
    # scoped packages
    access: github/org/Born-Studios
    publish: github/org/Born-Studios/team/Repo-Admins
    unpublish: github/org/Born-Studios/team/Repo-Admins
    # proxy: npmjs
    
    '**':
    # allow all users (including non-authenticated users) to read and
    # publish all packages
    #
    # you can specify usernames/groupnames (depending on your auth plugin)
    # and three keywords: "$all", "$anonymous", "$authenticated"
    access: github/org/Born-Studios

    # allow all known users to publish/publish packages
    # (anyone can register by default, remember?)
    publish:  github/org/Born-Studios/team/Repo-Admins
    unpublish:  github/org/Born-Studios/team/Repo-Admins

    # if package is not available locally, proxy requests to 'npmjs' registry
    # proxy: npmjs

# a long expiry is less secure but easier for developers
security:
  api:
#     legacy: true
    jwt:
      sign:
        expiresIn: 365d


max_body_size: 500mb

listen: 0.0.0.0:4873

url_prefix: '/'
```


## Verdaccio Service


`sudo nano /lib/systemd/system/verdaccio.service`


```
[Unit]
Description=Verdaccio lightweight npm proxy registry

[Service]
Type=simple
Restart=on-failure
User=azureuser
ExecStart=/home/azureuser/.nvm/versions/node/v18.16.1/bin/verdaccio --config /home/azureuser/verdaccio/config.yaml
Environment="VERDACCIO_PUBLIC_URL=https://upm.born.net"

[Install]
WantedBy=multi-user.target
```


### Fix node js not found error

`sudo ln -s "$(which node)" /usr/bin/node`

### Run the service

`sudo systemctl daemon-reload`

Don't start the service yet, we need certbot to spin up a server to obtain certificates and verdaccio will block port 80.


## SSL

Get your certificates.

`sudo certbot certonly`

Use option 1.

Note the outputs and use in the nginx configuration.

## Start service

`sudo systemctl start verdaccio`


## Nginx

`sudo nano /etc/nginx/sites-available/verdaccio.conf`

```
server {
    listen      80;
    server_name upm.born.net;
    return 301 https://$server_name$request_uri;
}


server {
    listen 443 ssl http2;
    server_name upm.born.net;
    
    ssl_certificate /etc/letsencrypt/live/upm.born.net/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/upm.born.net/privkey.pem;
    
    ssl on;
    ssl_session_cache  builtin:1000  shared:SSL:10m;
    ssl_protocols  TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!eNULL:!EXPORT:!CAMELLIA:!DES:!MD5:!PSK:!RC4;
    ssl_prefer_server_ciphers on;
    
    client_max_body_size 500M;
    
    location / {
        proxy_http_version 1.1;
        proxy_set_header    Upgrade $http_upgrade;
        proxy_set_header    Connection "upgrade";
        proxy_set_header    Host $host:$server_port;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto $scheme;
        proxy_pass          http://localhost:4873/;
        proxy_read_timeout  600;
        proxy_redirect off;
    }
    
    location ~ ^/verdaccio/(.*)$ {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $host:$server_port;
        proxy_set_header X-NginX-Proxy true;
        proxy_pass http://localhost:4873/$1;
        proxy_redirect off;
    }
}
```

`sudo ln /etc/nginx/sites-available/verdaccio.conf /etc/nginx/sites-enabled/`

`sudo systemctl restart nginx`
