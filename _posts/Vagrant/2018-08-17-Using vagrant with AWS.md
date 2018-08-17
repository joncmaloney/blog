---
layout: post
title: Using Vagrant with AWS
author: jon_maloney
summary: How to setup vagrant-aws and automate Launching EC2 instances.
---

source:

https://github.com/mitchellh/vagrant-aws
https://www.packer.io/docs/builders/amazon.html#toc_1
https://devops.com/devops-primer-using-vagrant-with-aws/

The latest version of vagrant-aws at the time of this blog post is 0.7.2.

#### Prerequisites 

If you haven't already done so the following pre-requisites are assumed for this tutorial. 
http://joncmaloney.com/blog/2018/08/17/Install-vagrant-Centos.html

First install vagrant-aws 

```bash
$ sudo vagrant plugin install vagrant-aws
# verify the install with 
$ vagrant plugin list
```

Download a dummy aws box and then update the parameters of the box. Essentially just use this file as a template. 

```bash
$ vagrant box add aws https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
```

When downloading this file I got an error message that seemed a bit weird. 

```bash
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'aws' (v0) for provider:
    box: Downloading: https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
    box: Download redirected to host: raw.githubusercontent.com
An error occurred while downloading the remote file. The error
message, if any, is reproduced below. Please fix this error and try
again.

The requested URL returned error: 502 Bad Gateway
```

If this happens just run the download again and it should be fine. 

```bash
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'dummy' (v0) for provider:
    box: Downloading: https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box
    box: Download redirected to host: raw.githubusercontent.com
==> box: Successfully added box 'dummy' (v0) for 'aws'!
```

Create a new IMA user so this user has access to creating ec2 instances. The following policy document provides the minimal set permissions necessary for Packer to work. There will be a series of blog posts about AWS security, users and roles but this is not yet complete until then you can check out [how packer.io has setup security for ec2 instances](https://www.packer.io/docs/builders/amazon.html#toc_1). The below is an extract of their security policy.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
      "Effect": "Allow",
      "Action" : [
        "ec2:AttachVolume",
        "ec2:AuthorizeSecurityGroupIngress",
        "ec2:CopyImage",
        "ec2:CreateImage",
        "ec2:CreateKeypair",
        "ec2:CreateSecurityGroup",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:CreateVolume",
        "ec2:DeleteKeyPair",
        "ec2:DeleteSecurityGroup",
        "ec2:DeleteSnapshot",
        "ec2:DeleteVolume",
        "ec2:DeregisterImage",
        "ec2:DescribeImageAttribute",
        "ec2:DescribeImages",
        "ec2:DescribeInstances",
        "ec2:DescribeRegions",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSnapshots",
        "ec2:DescribeSubnets",
        "ec2:DescribeTags",
        "ec2:DescribeVolumes",
        "ec2:DetachVolume",
        "ec2:GetPasswordData",
        "ec2:ModifyImageAttribute",
        "ec2:ModifyInstanceAttribute",
        "ec2:ModifySnapshotAttribute",
        "ec2:RegisterImage",
        "ec2:RunInstances",
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource" : "*"
  }]
}
```

After running "*vagrant box add...*" a Vagrantfile will appear in your current directory. Modify the Vagrantfile so it looks similar to the blow. Note there are some extended parameters that are not included in the blog post from [mitchellh](https://github.com/mitchellh/vagrant-aws) or from [devops.com](https://devops.com/devops-primer-using-vagrant-with-aws/). These extended parameters are to ensure vagrant can access the new ec2 instance via our VPC. If you are not sure about how to setup EC2 infrastructure check out our upcoming series.

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Require the AWS provider plugin
require 'vagrant-aws'

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "aws"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "aws" do |aws, override|
    # Read AWS authentication information from environment variables
    aws.access_key_id = 'XXXXXXXXXXXXXXXXXXX'
    aws.secret_access_key = 'XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX'
    # Specify SSH keypair to use
    aws.keypair_name = 'MyPemFile'
    aws.instance_type = 't2.micro'
    # Specify region, AMI ID, and security group(s)
    aws.region = 'ap-southeast-2'
    aws.ami = 'ami-67589505'
    aws.security_groups = ['sg-f1d64889']
    aws.subnet_id = 'subnet-2477d77c'
    aws.associate_public_ip = 'true'
    # Specify username and private key path
    override.ssh.username = 'ec2-user'
    override.ssh.private_key_path = "Path to my ssh pem file"
    aws.ssh_host_attribute = :private_ip_address
    # Tag the machine with a name.
    aws.tags = {
          'Name' => 'my-server'
    }

    # Customize the amount of memory on the VM:
    # vb.memory = "1024"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    yum -y update
    yum install -y wget nano git
    yum install -y yum-utils device-mapper-persistent-data lvm2
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install -y http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.42-1.gitad8f0f7.el7.noarch.rpm
    yum install -y docker-ce
    systemctl start docker
  SHELL
end
```

#### Troubleshoot: 

To help troubleshoot when using vagrant. You can use the --debug option which will show debug logs whilst vagrant is statrting up. 

```bash
$ sudo vagrant up --debug --provider=aws
```

