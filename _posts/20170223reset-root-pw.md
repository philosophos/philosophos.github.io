---
layout: post
title: reset root pw
date: 2017-02-23 00:10:30
updated:
copyright: true
toc: false
description: Linux | root | password
categories: Linux
tags:
- Linux
- root
- password
---

**rpm-based Distros**(RedHat,CentOS,Fedora)
**deb-baesd Distros**(Debian,Ubuntu,Mint)
If you enter "passwd"or"vi"or"vim",then may show "command not found". because /usr isn't mount.
If you `/usr` is in LVM, you can't mount it because `lvm` is in `/usr` not in kernel. You need use `nano`,which is included in initramfs.
<!-- more -->

-------------------------------------------------------------------------------
**rpm-based Distros**(RedHat,CentOS,Fedora)
boot to GRUB menu,choose the bar,push "e",change "ro" to "rw init=/sysroot/bin/sh",push 'Ctrl+X'or'F10' to boot as root

    #chroot /sysroot
    #passwd              ## enter root password
    #touch /.autorelabel ## update SELinux info
    #exit
    #reboot

-------------------------------------------------------------------------------
**deb-baesd Distros**(Debian,Ubuntu,Mint)
boot to GRUB menu,choose the bar,push "e",change "ro" to "rw init=/bin/bash",then push 'Ctrl+X'or'F10' to boot as root
then enter `passwd` to reset root password

-------------------------------------------------------------------------------
If you enter "passwd"or"vi"or"vim",then may show "command not found". because /usr isn't mount.

    #cat /etc/fstab              ## (find the paratition with /usr
    #mount /dev/"para_name" /usr ## (mount /usr;para_name like sda8
    #passwd root                 ## (reset root password
    #sync                        ## (update info

-------------------------------------------------------------------------------
If you `/usr` is in LVM, you can't mount it because `lvm` is in `/usr` not in kernel. Then you need use the very lightweight(about 200KB) editor `nano`,which is included in initramfs.

    #cd /etc
    #cp shadow shadow.bak ## (backup the shadows file
    #nano shadow          ## (edit shadow with nano
the root's info on the first line,del the shadow,push 'Ctrl+X',choose yes to save reboot,then the root can be log without password.Remenber run `passwd`!

-----------------------------------------------------------
