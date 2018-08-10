---
layout: post
title: Install Docker CE - CentOS
author: jon_maloney
summary: A summary of how to install Docker CE on a clean CentOS build. 
---

source: https://docs.docker.com/install/linux/docker-ce/centos/

Install using the repository
-----------------------------
Before you install Docker CE for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker

1. Install required packages. yum-utils provides the yum-config-manager utility, and device-mapper-persistent-data and lvm2 are required by the devicemapper

  ```bash
  $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
  ```

2. Use the following command to set up the stable repository. You always need the stable repository, even if you want to install builds from the edge or test 

  ```bash
   $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  ```

3. Depending on your base linux image the package container-selinux may need to be installed.

   ```bash
   $ sudo yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.42-1.gitad8f0f7.el7.noarch.rpm
   ```

Install Docker CE
------------------

1. Install the latest version of Docker CE, or go to the next step to install a specific version:

  ```bash
  $ sudo yum install -y docker-ce
  ```

2. Start docker daemon
   
  ```bash
  $ sudo systemctl start docker
  ```
