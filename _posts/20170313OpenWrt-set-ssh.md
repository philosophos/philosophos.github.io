---
layout: post
title: OpenWrt -- Set SSH
Copyright: true
date: 2017-03-13 20:28:31
updated:
description: On OpenWrt , set ssh Signature Authentication, disable PasswordAuth, change default port.
categories: 
- Linux 
- OpenWrt
tags:
- Linux
- OpenWrt
- SSH
- Signature Authentication
- port
---

- Set Password At First Login
- SSH Signature Authentication
    - Authentication Key Generation
    - Copy Public Key To The OpenWrt System
- Disable PasswordAuth, Change Default Port
    - Use uci command
    - Edit /etc/config/dropbear Directly
- Simple Your Life With an SSH Config File

About [dropbear](https://matt.ucc.asn.au/dropbear/dropbear.html)
<!-- more -->

-----------------------------------------------------------
Set Password At First Login
-----------------------------------------------------------

    $ telnet 192.168.1.1
    telnet 192.168.1.1
    Trying 192.168.1.1...
    Connected to 192.168.1.1.
    Escape character is '^]'.
     === IMPORTANT ============================
      Use 'passwd' to set your login password
      this will disable telnet and enable SSH
     ------------------------------------------


    BusyBox v1.23.2 (2016-01-02 05:45:02 CET) built-in shell (ash)

      _______                     ________        __
     |       |.-----.-----.-----.|  |  |  |.----.|  |_
     |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
     |_______||   __|_____|__|__||________||__|  |____|
              |__| W I R E L E S S   F R E E D O M
     -----------------------------------------------------
     CHAOS CALMER (15.05.1, r48532)
     -----------------------------------------------------
      * 1 1/2 oz Gin            Shake with a glassful
      * 1/4 oz Triple Sec       of broken ice and pour
      * 3/4 oz Lime Juice       unstrained into a goblet.
      * 1 1/2 oz Orange Juice
      * 1 tsp. Grenadine Syrup
     -----------------------------------------------------
    root@openwrt:~$ passwd
    Changing password for root
    New password:
    Retype password:
    Password for root changed by root
    root@OpenWrt:~$
- Please choose a secure password, else the passwd tool will say to you `Bad password: too weak` after the line `New password: `
- After you set a password the telnet daemon will be disabled, type exit into the prompt Without reboot, SSH is now available; so is HTTPS if the WebUI (LuCI) is installed with it's TLS-modules


    $ telnet 192.168.1.1
    Trying 192.168.1.1...
    Connected to 192.168.1.1.
    Escape character is '^]'.
    Login failed.
    Connection closed by foreign host

    $ ssh root@192.168.1.1
    The authenticity of host '192.168.1.1 (192.168.1.1)' can't be established.
    RSA key fingerprint is SHA256:VWZOi9dHQ06YSQuLVg0i74MH7HhJj118KWe+dixtZvI.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '192.168.1.1' (RSA) to the list of known hosts.
    root@192.168.1.1's password:


    BusyBox v1.23.2 (2016-01-02 05:45:02 CET) built-in shell (ash)

      _______                     ________        __
     |       |.-----.-----.-----.|  |  |  |.----.|  |_
     |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
     |_______||   __|_____|__|__||________||__|  |____|
              |__| W I R E L E S S   F R E E D O M
     -----------------------------------------------------
     CHAOS CALMER (15.05.1, r48532)
     -----------------------------------------------------
      * 1 1/2 oz Gin            Shake with a glassful
      * 1/4 oz Triple Sec       of broken ice and pour
      * 3/4 oz Lime Juice       unstrained into a goblet.
      * 1 1/2 oz Orange Juice
      * 1 tsp. Grenadine Syrup
     -----------------------------------------------------
    root@OpenWrt:~#


-----------------------------------------------------------
SSH Signature Authentication
-----------------------------------------------------------
#### Authentication Key Generation

    $ ssh-keygen -C 'root@WRT' -f ~/.ssh/wrt_rsa
    Generating public/private rsa key pair.
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /home/j0ham/.ssh/wrt_rsa.
    Your public key has been saved in /home/j0ham/.ssh/wrt_rsa.pub.
    The key fingerprint is:
    SHA256:MKm5X6uxA9VM9iNVQNEA4A7fpumL8KTsL820pxrSzGk root@WRT
    The key's randomart image is:
    +---[RSA 2048]----+
    |      ...o=*.    |
    |     . .o . .    |
    |    . == o       |
    |     *.++ o      |
    |    o.o S. .     |
    | + .o. +         |
    |. E+oo+ .        |
    | +.*++o+ .       |
    | .=+=o*+.        |
    +----[SHA256]-----+

#### Copy Public Key To The OpenWrt System

{% codeblock lang:sh %}
$ cat ~/.ssh/wrt_rsa.pub | ssh root@192.168.1.1 'cat >> /etc/dropbear/authorized_keys; chmod 0600 /etc/dropbear/authorized_keys'
{% endcodeblock %}

Now `ssh -i ~/.ssh/wrt_rsa root@192.168.1.1` can ssh to the OpenWrt system.


-----------------------------------------------------------
Disable PasswordAuth, Change Default Port
-----------------------------------------------------------
#### Use uci command

    root@OpenWrt:~# uci show dropbear
    dropbear.@dropbear[0]=dropbear
    dropbear.@dropbear[0].PasswordAuth='on'
    dropbear.@dropbear[0].RootPasswordAuth='on'
    dropbear.@dropbear[0].Port='22'
    root@OpenWrt:~# uci set dropbear.@dropbear[0].Port='2222'
    root@OpenWrt:~# uci set dropbear.@dropbear[0].PasswordAuth='off'
    root@OpenWrt:~# uci set dropbear.@dropbear[0].RootPasswordAuth='off'
    root@OpenWrt:~# uci commit dropbear
    root@OpenWrt:~# uci show dropbear
    dropbear.@dropbear[0]=dropbear
    dropbear.@dropbear[0].Port='2222'
    dropbear.@dropbear[0].PasswordAuth='off'
    dropbear.@dropbear[0].RootPasswordAuth='off'
    root@OpenWrt:~# /etc/init.d/dropbear reload

#### Edit /etc/config/dropbear Directly

    root@OpenWrt:~# cat /etc/config/dropbear
    config dropbear
        option PasswordAuth 'on'
        option RootPasswordAuth 'on'
        option Port         '22'
    #   option BannerFile   '/etc/banner'

{% codeblock lang:sh Edit /etc/config/dropbear %}
config dropbear
        option PasswordAuth 'off'
        option RootPasswordAuth 'off'
        option Port         '2222'
#       option BannerFile   '/etc/banner'
{% endcodeblock %}

    root@OpenWrt:~# /etc/init.d/dropbear reload

Now only `ssh -i ~/.ssh/wrt_rsa -p 2222 root@192.168.1.1` can ssh to the OpenWrt system.

-----------------------------------------------------------
Simple Your Life With an SSH Config File
-----------------------------------------------------------
{% codeblock lang:sh Edit ~/.ssh/config %}
Host openWRT
    User root
    Hostname 192.168.1.1
    Port 2222
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/wrt_rsa
{% endcodeblock %}

Now `ssh openWRT` can ssh to the OpenWrt system.

-----------------------------------------------------------
About [dropbear](https://matt.ucc.asn.au/dropbear/dropbear.html)
Dropbear is a relatively small SSH server and client. It runs on a variety of POSIX-based platforms. Dropbear is open source software, distributed under a MIT-style license.  Dropbear is particularly useful for "embedded"-type Linux (or other Unix) systems, such as wireless routers.

