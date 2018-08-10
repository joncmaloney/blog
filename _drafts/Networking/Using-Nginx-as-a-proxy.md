---
layout: post
title: Using Niginx as a proxy
summary: How to setup nginx using docker and configuring a proxy
author: jon_maloney
---

First create a docker compose file

```bash
$ nano ./docker-compose.yml 
```

```yaml
version: '2'

services:
  nginx-proxy:
    container_name: nginx
    image: jwilder/nginx-proxy:alpine
    ports:
      - "80:80"
    volumes:
      - ./reverse_proxy.conf:/etc/nginx/conf.d/my_proxy.conf:ro
      - ./proxy.conf:/etc/nginx/proxy.conf:ro
      - /var/run/docker.sock:/tmp/docker.sock:ro
```

We will create a file for configuring the proxy reverse_proxy.conf and a file for proxy settings. The proxy settings describe how nginx will map http headers during proxy requests and reverse_proxy describes how to route http requests. 

```bash
$ nano ./reverse_proxy.conf
```

```
map $http_upgrade $connection_upgrade {
    default    upgrade;
    ''         close;
}

server {
    listen       80 default_server;
    server_name  joncmaloney.com;
    
    location / {
        set $jekyll_upstream "http://jekyll-web-server:4000";
        include /etc/nginx/proxy.conf;
        proxy_pass $jekyll_upstream;
    }
}
```

```bash
$ nano ./proxy.conf
```

```
# HTTP 1.1 support
proxy_http_version 	1.1;
proxy_buffering 	off;
proxy_redirect     	off;
proxy_set_header 	Host 			$host;
proxy_set_header 	Upgrade 		$http_upgrade;
proxy_set_header	Connection 		$connection_upgrade;
proxy_set_header 	X-Forwarded-Proto 	$scheme;
proxy_set_header 	X-Real-IP 		$remote_addr;
proxy_set_header 	X-Forwarded-For 	$proxy_add_x_forwarded_for;
```

Run the docker compose file

```bash
$ docker-compose up -d
```

