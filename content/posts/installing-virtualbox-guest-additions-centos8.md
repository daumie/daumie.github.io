---
title: "Installing Virtualbox Guest Additions Centos8"
date: 2019-10-17T12:35:18+03:00
draft: false
toc: false
images:
tags: 
  - centos
  - linux
  - redhat
  - virtualbox
---

From [tecmint](https://www.tecmint.com/install-virtualbox-guest-additions-in-ubuntu/) ; VirtualBox Guest Additions
are a collection of device drivers and system applications designed to achieve closer integration between the host
and guest operating systems. They help to enhance the overall interactive performance and usability of guest systems.

The VirtualBox Guest Additions offer the following features:

+ Easy mouse pointer integration.
+ Easy way to share folders between the host and the guest.
+ Drag and drop feature allows copying or opening files, copy clipboard formats from the host to the guest or from the guest to the host.
+ Share clipboard (for copy and paste) of the guest operating system with your host operating system.
+ Better video support provides accelerated video performance.
+ Better Time synchronization between guest and host.
+ Standard host/guest communication channels.

### Installing Guest Additions on CentOs 8

CentOS is a Linux distribution that provides a free, community-supported computing platform functionally compatible with its upstream source, Red Hat Enterprise Linux. 

#### Install build depencies

##### Step1: Change to root user

```bash
sudo -i
or
su -
```

#### Step2: Make sure you are using the latest kernel

```bash
# yum update
```

**Reboot

#### Step3: Install Packages necessary for build

```bash
# yum install gcc make kernel-devel kernel-headers elfutils-libelf-devel bzip2 perl
```

#### Step4: Mount guest additons iso

#### Step5: Install guest additions

```bash
# cd /media/VirtualBoxGuestAdditions
```

Then run

```bash
# ./VBoxLinuxAdditions.run
```

#### Step6: Reboot guest system

```bash
# reboot now
```
