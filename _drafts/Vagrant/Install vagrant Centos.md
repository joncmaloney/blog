# Install Vagrant on CentOS

source: 

https://www.tecmint.com/how-to-install-vagrant-on-centos-7/



In this tutorial version 2.1.2 of vagrant has been used. To find the latest version go to https://www.vagrantup.com/downloads.html.

I've used https://releases.hashicorp.com/vagrant/2.1.2/vagrant_2.1.2_x86_64.rpm

For this tutorial we assume that nano and wget are installed. Also best practice when starting off from a blank OS to run an update. 

```bash
$ sudo yum -y update && sudo yum -y install wget nano
```

I've had some trouble installing vagrant on the EC2 free tier Red Hat Enterprise version. I eventually got it working by first installing vagrant from the rpm then installing ancillary packages. First download the rpm then install. To verify the install was successful run vagrant -v. 

```bash
# download the rpm
$ sudo wget https://releases.hashicorp.com/vagrant/2.1.2/vagrant_2.1.2_x86_64.rpm
# install the rpm
$ sudo yum install vagrant_2.1.2_x86_64.rpm
# check the install was successful by printing the version
$ vagrant -v
Vagrant 2.1.2
```

Then I install the ancillary packages. This seems to be the wrong order. But it worked for me.

```bash
$ sudo yum -y install gcc dkms make qt libgomp patch
$ sudo yum -y install kernel-headers kernel-devel binutils glibc-headers glibc-devel font-forge
```

To start a new vm using vagrant. Create a working directory in an appropriate place, initialise your VM then start the virtual machine. 

```bash
$ mkdir ~/vagrant
$ cd ~/vagrant

# initialise the virtual machine
$ vagrant init centos/7

# this creates the vagrantfile
$ ls
Vagrantfile

#star the vm
$ vagrant up
```

To connect to your new virtual machine. 

```bash
$ vagrant ssh
```



Then install the vagrant-aws plugin