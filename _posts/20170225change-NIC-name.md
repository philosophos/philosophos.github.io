---
layout: post
title: change NIC name
date: 2017-02-25 23:43:19
updated:
description: change network interface card name
categories: Linux
tags:
- Linux
- Network
---

===========================================================
- Change grub.cfg
- Change udev rules
- Change Network Interface Card Configuration
<!-- more -->

-----------------------------------------------------------
Change grub.cfg
-------------------------------------------------------------------------------
**rpm-based:**
`vim /etc/sysconfig/grub`
Find the line including `GRUB_CMDLINE_LINUX`,add `net.ifnames=0 biosdevname=0` in front of `rhgb`

`grub2-mkconfig -o /boot/grub2/grub.cfg`

**deb-based/archlinux:**
`vim /etc/default/grub`
Find the line including `GRUB_CMDLINE_LINUX`,add `net.ifnames=0 biosdevname=0`

`grub-mkconfig -o /boot/grub/grub.cfg`

-----------------------------------------------------------
Change udev rules
-------------------------------------------------------------------------------
mv /etc/udev/rules.d/70-persistent-ipoib.rules /etc/udev/rules.d/70-persistent-ipoib.rules.bak

vim /etc/udev/rules.d/10-network.rules
`SUBSYSTEM=="net",ACTION=="add",ENV{ID_NET_NAME_MAC}=="enx000c2926e8bc",NAME="eth0"`
`SUBSYSTEM=="net",ACTION=="add",ATTR{address}=="00:01:02:8c:50:09",NAME="eth1"`
**NOTE**
`cat /sys/class/net/<device-name>/address` can get MAC addr
use the lower-case hex values in MAC addr 

-----------------------------------------------------------
Change Network Interface Card Configuration
-------------------------------------------------------------------------------
`cd /etc/sysconfig/network-scripts/`
`mv ifcfg-eno\* ifcfg-eth\*`
`vim ifcfg-eth\*`
`NAME=eth0`

