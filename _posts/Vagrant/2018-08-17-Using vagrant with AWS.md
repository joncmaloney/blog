---
layout: post
title: Using Vagrant with AWS
author: jon_maloney
summary: How to setup vagrant-aws and automate Launching EC2 instances.
---

source:
https://github.com/mitchellh/vagrant-aws

The latest version of vagrant-aws at the time of this blog post is 0.7.2.

#### Prerequisites 

If you haven't already done so the following pre-requisites are assumed for this tutorial. 

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

