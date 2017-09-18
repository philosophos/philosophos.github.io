---
layout: post
title: set PPPOE connection
copyright: true
date: 2017-04-10 18:50:21
updated:
description:
categories:
- Linux
- Network
tags:
- Linux
- Network
- PPPOE
---

- deb-based Distro
- rpm-based Distro
- Arch Linux
<!-- more -->

-----------------------------------------------------------
deb-based Distro
-----------------------------------------------------------
### Install

    apt-get install pppoe pppoeconf

### Configure

    pppoeconf

### Use

    pon dsl-provider
    poff

### configuration files
/etc/sysconfig/network-scripts/ifcfg-eth0
/etc/network/interfaces
/etc/ppp/peers/dsl-provider
/etc/ppp/chap-secrets
/etc/ppp/pap-secrets

-----------------------------------------------------------
rpm-based Distro
-----------------------------------------------------------
### Install

    yum install rp-pppoe
or

    rpm -ivh /some-path/rp-pppoe-3.11-5.el7.x86_64.rpm

### Configure

    pppoe-setup

### Use

    pppoe-status /etc/sysconfig/network-scripts/ifcfg-ppp0
    pppoe-start
    pppoe-stop
    ifup ppp0
    ifdown ppp0

### configuration files
/etc/sysconfig/network-scripts/ifcfg-eth0
/etc/sysconfig/network-scripts/ifcfg-ppp0
/etc/ppp/chap-secrets
/etc/ppp/pap-secrets

-----------------------------------------------------------
Arch Linux
-----------------------------------------------------------
### Install

    pacman -S rp-pppoe

### Configure

    pppoe-setup

### Use

    pppoe-status
    pppoe-start
    pppoe-stop
    ifup ppp0
    ifdown ppp0

**To make pppoe auto run when booting up**
`systemctl enable adsl`

### configuration files
/etc/ppp/firewall-masq
/etc/ppp/firewall-standalone
/etc/ppp/pppoe-server-options
/etc/ppp/pppoe.conf

