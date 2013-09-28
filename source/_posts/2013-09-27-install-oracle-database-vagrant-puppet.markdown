---
layout: post
title: "Installing Oracle database in Vagrant VM using Puppet"
date: 2013-09-27 18:31
comments: true
categories: 
- Oracle
- Vagrant
- Puppet
---
[Vagrant](http://www.vagrantup.com/) enbles creating virtual machines easily, [Puppet](http://puppetlabs.com/) helps to setup Linux machine. In this example we will install Oracle database on Oracle Enterprise Linux Vagrant box using Puppet. Before we continue, Vagrant must be setup on your machine. There are a lot of tutorials on how to install Vagrant, one of the best is [Vagrant official site](http://www.vagrantup.com/). Also you need [VirtualBox](https://www.virtualbox.org/) installed. Once you can see your Vagrant version (using `vagrant --version` command) and VirtualBox installed you can continue with this tutorial. 
So, in order to create new VM with Oracle database follow these steps:

1. Download and unzip file from [here](https://github.com/paykin/vagrant-oracle-11g/archive/master.zip) or clone it from my GitHub repo `https://github.com/paykin/vagrant-oracle-11g` to your hard disk.

2. Download Oracle Database installation from [Oracle downloads site](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html). For this tutorial we need **Oracle Database 11g Release 2 (11.2.0.1.0)** for Linux 64bit (Linux x86-64). There are 2 files to download. You need to place them to `vagrant-oracle-11g/modules/oracle/files` directory. While it is downloading, proceed to next step.

3. In `Vagrant` file you may set machine host name, amount of memory
4. In `manifests/base.pp` you can set Oracle database parameters

4. In command line, go to directory you extracted attached file and write `vagrant up`.

That's it. Installation will take a while, on my machine it too about 10 minutes. When it finishes, you can connect to the machine via SSH, issuing `vagrant ssh` command. If you prefer using standard ssh, you can connect to it with `ssh vagrant@localhost -p 2222` command. Vagrant default root password is `vagrant`.
In order to validate that everything works, you can run `sqlplus / as sysdba` and execute some simple query, say `SELECT SYSDATE FROM dual`. If you see current date - everything works ok. Also you can connect using SqlDeveloper or any othe client.
