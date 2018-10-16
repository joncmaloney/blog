---
layout: post
title: Creating an AWS Bastion Host Using Terraform
author: jon_maloney
summary: A simple to follow tutorial on building a bastion / jump host using terraform.
---
Reference:
https://github.com/clstokes/example-terraform-bastion

Source:
https://github.com/joncmaloney/terraform

#### Prerequisites 

First install Terraform. If you have not done so already here is a quick guide to installing terraform on windows. 

http://www.joncmaloney.com/blog/2018/08/25/Getting-Started-Install-Terraform-on-Windows.html/

It's not really any point creating a bastion unless you are also creating a server that you connect to via the bastion. This tutorial we will create two servers; a bastion and a simple web-server. We will cover how to create a bastion, how to create a secure VPC network, setup appropriate firewall rules and connecting to the web-server via the bastion.

## Create AWS IAM for this project

Create a role for this user. Including the AWS managed policies. These permissions will allow this user to create EC2 instances and to create networks in VPC. 

1. AmazonEC2FullAccess
2. AmazonVPCFullAccess

Note down the access key and secret; these will be used in the next section.

Next create a private key that will be used to SSH into your host.

## Setting up the instances

First lets create two simple instances. We won't do anything with them yet just spin them up. Both these servers will have public IP addresses. At this stage you will have to configure AWS Security Group manually if you want to get access to the machine. We will use Terraform to configure this later in the tutorial. 

Create a file ending with .tf i've used 'aws-instances.tf'.

```json
provider "aws" {
  access_key = "YOUR_ACCESS_KEY"
  secret_key = "YOUR_SECRET_KEY"
  region     = "YOUR_AWS_REGION"
}

resource "aws_instance" "ssh_bastion" {
  instance_type = "t2.micro"
  ami = "ami-67589505"
  key_name = "YOUR_PRIVATE_KEY_NAME"
  associate_public_ip_address = true
}

resource "aws_instance" "web_server" {
  instance_type = "t2.micro"
  ami = "ami-67589505"
  key_name = "YOUR_PRIVATE_KEY_NAME"
  associate_public_ip_address = true
}
```

This file will use the aws provider to create the two ec2 instances. Before you can apply the configuration you will need to init terraform to download the aws provider. 

```bash
$ terraform init
```

Apply the terraform configuration. 

```bash
$ terraform apply
```

If you log into your AWS EC2 dashboard you should see two new instances started up. 

## Add Terraform Variables

In the above example we hard coded keys into the instances file. This is not going to very secure. We can define keys as variables in seperate file. Stary by creating a file called 'secrets.tf'

```json
variable "aws_access_key" {
  default = "YOUR_ACCESS_KEY"
}

variable "aws_secret_key" {
  default = "YOUR_SECRET_KEY"
}
```

You can then update your instances file and use these variables. 

```json
provider "aws" {
  access_key = "${var.aws_access_key}"
  secret_key = "${var.aws_secret_key}"
}
```

I've also added the secret.tf file into .gitignore to ensure the secrets file isn't added to source control. 

```
secrets.tf
```

## Setup AWS VPC

```json
# Create a VPC to launch our instances into
resource "aws_vpc" "terraform" {
  cidr_block = "${var.vpc_cidr}"

  tags {
    Name = "${var.project_name}"
  }
}

# Create an internet gateway to give our subnet access to the outside world
resource "aws_internet_gateway" "terraform" {
  vpc_id = "${aws_vpc.terraform.id}"

  tags {
    Name = "${var.project_name}"
  }
}

resource "aws_route_table" "internet" {
  vpc_id = "${aws_vpc.terraform.id}"

  tags {
    Name = "${var.project_name}-internet"
  }
}

# Grant the VPC internet access
resource "aws_route" "internet_access" {
  route_table_id         = "${aws_route_table.internet.id}"
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = "${aws_internet_gateway.terraform.id}"
}

# Create an association between the VPC and route table
resource "aws_main_route_table_association" "terraform" {
  vpc_id         = "${aws_vpc.terraform.id}"
  route_table_id = "${aws_route_table.internet.id}"
}

# Create a subnet to launch web servers into
resource "aws_subnet" "terraform_web_servers" {
  vpc_id                  = "${aws_vpc.terraform.id}"
  cidr_block              = "${var.vpc_cidr_web_servers}"
  map_public_ip_on_launch = true

  tags {
    Name = "${var.project_name}"
  }
}
```