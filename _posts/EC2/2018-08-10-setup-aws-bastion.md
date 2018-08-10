
---
layout: post
title: Setup bastion host in AWS
author: jon_maloney
summary: Secure your private cloud by setting up a bastion host or jump host.
---

Source: 
https://aws.amazon.com/blogs/security/securely-connect-to-linux-instances-running-in-a-private-amazon-vpc/

The jump host is a dedicated locked down EC2 instance that is only accessable via port 22. Once connected to this instance you use this connection to connect to other instances on your private cloud. This way only one server is accessable to ssh connections publicly. All other ssh sessions are locked down to connections on the private network only. 

Reducing attack surface. 

Each EC2 instance doesn't need to have port 22 open. Further an publicly open port 22 can leave your EC2 instance vulneerable to attack even if the attacker can't get access to the machine. 

