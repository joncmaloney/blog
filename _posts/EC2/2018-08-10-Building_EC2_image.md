---
layout: posts
title: Building EC2 image
summary: Quick reference summary of how I've built my EC2 instances.
author:jon_maloney
---


```bash
  $ sudo yum update -y && install -y nano wget yum-utils device-mapper-persistent-data lvm2
  $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  $ sudo yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.42-1.gitad8f0f7.el7.noarch.rpm
  $ sudo yum install -y docker-ce
  $ sudo systemctl start docker
  sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  chmod +x /usr/local/bin/docker-compose
  cd /etc/bash_completion.d/.
  curl -L https://raw.githubusercontent.com/docker/compose/1.22.0/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
  
```
