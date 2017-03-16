---
layout: post
title: Installing BlackArch on top of Arch Linux
copyright: true
date: 2017-03-05 13:25:58
updated:
description: Install | BlackArch
categories: Linux
tags:
- Linux
- BlackArch
- Install
---

- Add [blackarch] Repositories To /etc/pacman.conf
- Install & Setup BlackArch Linux Keyring
- Install & Setup ruby-bundler
- Install Tools From The Blackarch Repository
- Configure Menus
    - automatic generate WM menu from xdg files
    - For awesome 4
- Build The Blackarch Packages From Source

<!-- more -->

-------------------------------------------------------------------------------
 Add [blackarch] Repositories To /etc/pacman.conf
-------------------------------------------------------------------------------
{% codeblock lang:sh Add the following to /etc/pacman.conf %}
    [blackarch]
    #SigLevel = Optional TrustAll
    Server=https://mirrors.tuna.tsinghua.edu.cn/blackarch/blackarch/os/$arch
    Server=https://mirrors.ustc.edu.cn/blackarch/blackarch/os/$arch
{% endcodeblock %}
**NOTE**: These mirrors are for Chinese user,other countris' users see [Official BlackArch Linux Mirror Sites](https://www.blackarch.org/downloads.html#mirror-list)

-------------------------------------------------------------------------------
 Install & Setup BlackArch Linux Keyring
-------------------------------------------------------------------------------
{% codeblock lang:sh %}
curl -O https://blackarch.org/strap.sh
sha1sum strap.sh  # The SHA1 sum should match: 34b1a3698a4c971807fb1fe41463b9d25e1a4a09
bash strap.sh     # Run strap.sh as root
{% endcodeblock %}

-------------------------------------------------------------------------------
 Install & Setup ruby-bundler (Optional)
-------------------------------------------------------------------------------
{% codeblock lang:sh %}
sudo pacman -S ruby-bundler
su -c "bundle config 'mirror.https://rubygems.org' 'https://gems.ruby-china.org'"
{% endcodeblock %}
**ruby gem mirrors for Chinese users:**
https://gems.ruby-china.org
http://ruby.taobao.com
http://mirrors.aliyun.com/rubygems/
http://mirrors.tuna.tsinghua.edu.cn/rubygems/

-------------------------------------------------------------------------------
 Install Tools From The Blackarch Repository
-------------------------------------------------------------------------------
{% codeblock lang:sh %}
# To list all of the available tools, run
$ sudo pacman -Sgg | grep blackarch | cut -d' ' -f2 | sort -u

# To install all of the tools, run
$ sudo pacman -S blackarch

# To install a category of tools, run
$ sudo pacman -S blackarch-<category>

# To see the blackarch categories, run
$ sudo pacman -Sg | grep blackarch 
{% endcodeblock %}

Most package in the blackarch repository is also in blackarch group and blackarch-<category> group,but there are still some package not.  Run this script for more info.

{% codeblock lang:sh %}
#!/bin/bash
    tool_grp=$(pacman -Sg|grep blackarch-)
    echo '--------------------------------------------------------------------------------'
    echo -e "packages not in either blackarch group or blackarch-<category> group,\n\
    won't be installed by \e[1mpacman -S blackarch-<category>\e[0m or \e[1mpacman -S blackarch\e[0m"
    comm -23 <(comm -3 <(pacman -Ssq ^blackarch|sort) <(pacman -Sgq blackarch|sort)) \
    <(pacman -Sgq $tool_grp|sort -u)
    echo '--------------------------------------------------------------------------------'
    echo -e "packages in blackarch group but not in blackarch-<category> group,\n\
    won't be installed by \e[1mpacman -S blackarch-<category>\e[0m"
    comm -23 <(pacman -Sgq blackarch|sort) <(pacman -Sgq $tool_grp|sort -u)
    echo '--------------------------------------------------------------------------------'
    echo -e "packages not in blackarch group but in blackarch-<category> group,\n\
    won't be installed by \e[1mpacman -S blackarch\e[0m"
    comm -13 <(pacman -Sgq blackarch|sort) <(pacman -Sgq $tool_grp|sort -u)
    echo '--------------------------------------------------------------------------------'
{% endcodeblock %}
It will output like following:

    --------------------------------------------------------------------------------
    packages not in either blackarch group or blackarch-<category> group,
        won't be installed by pacman -S blackarch-<category> or pacman -S blackarch
    blackarch-config-awesome
    blackarch-config-bash
    blackarch-config-fluxbox
    blackarch-config-gtk
    blackarch-config-lxdm
    blackarch-config-openbox
    blackarch-config-spectrwm
    blackarch-config-vim
    blackarch-config-wmii
    blackarch-config-x11
    blackarch-devtools
    blackarch-dwm
    blackarch-installer
    blackarch-keyring
    blackarch-wallpaper
    blackmate
    --------------------------------------------------------------------------------
    packages in blackarch group but not in blackarch-<category> group,
        won't be installed by pacman -S blackarch-<category>
    blackarch-menus
    blackarch-mirrorlist
    ruby-msgpack
    --------------------------------------------------------------------------------
    packages not in blackarch group but in blackarch-<category> group,
        won't be installed by pacman -S blackarch
    cryptohazemultiforcer
    cudahashcat
    malboxes
    truecrack
    vmcloak
    --------------------------------------------------------------------------------
Install the package you need.

-------------------------------------------------------------------------------
 Configure Menus
-------------------------------------------------------------------------------
You may have customized WM or DE, only need a menu for BlackArch rather than installing package `blackarch-config-*`

### automatic generate WM menu from xdg files

    pacman -S archlinux-xdg-menu blackarch-menus

`xdg_menu` output format can be:
 twm, WindowMaker, fvwm2, icewm, ion3, blackbox, fluxbox, openbox, xfce4, openbox3, openbox3-pipe, awesome jwm
And it compatible with Gnome, KDE, Xfce and Enlightenment.
For more info about xdg_menu usage & examples, see [ArchWiki](https://wiki.archlinux.org/index.php/Xdg_menu)

### For awesome 4
If you use awesome 4, the xdg_menu does not create the correct lua code.But as a workaround you can do as following:

{% codeblock lang:sh %}
    pacman -Sw blackarch-config-awesome
    tar xJf /var/cache/pacman/pkg/blackarch-config-awesome-*.pkg.tar.xz -C /tmp etc/xdg/awesome/rc.lua.blackarch
    sed -n '/antiforensicmenu/,/mymainmenu/p' /tmp/etc/xdg/awesome/rc.lua.blackarch \
    |sed -n '$d;w ~/.config/awesome/blackarchmenu.lua'
    rm -r /tmp/etc
{% endcodeblock %}
Then edit you rc.lua as shown below:
    Add a require statment for your new menu.lua file
    Add an entry to your awful.menu object for your new menu which calls xdgmenu

{% codeblock lang:lua %}
    ...
    xdg_menu = require("blackarchmenu")
    ...
    
    ...
    mymainmenu = awful.menu({ items = { { "awesome", myawesomemenu, beautiful.awesome_icon },
                                        { "BlackArch", blackarchmenu },
                                        { "open terminal", terminal }
                                        ...
                                      }
                            })
    ...
{% endcodeblock %}

-------------------------------------------------------------------------------
 Build The Blackarch Packages From Source
-------------------------------------------------------------------------------
As part of an alternative method of installation, you can build the blackarch packages from source. You can find the PKGBUILDs on github. To build the entire repo, you can use the blackman tool.

{% codeblock lang:sh %}
# First, you must install blackman. If the BlackArch package repository is setup on your machine,
# you can install blackman like:
$ sudo pacman -S blackman

# Download, compile and install package:
$ sudo blackman -i <package>

# Download, compile and install whole category
$ sudo blackman -g <group>

# Download, compile and install all BlackArch tools
$ sudo blackman -a

# To list blackarch categories
$ blackman -l

# To list category tools
$ blackman -p <category>
{% endcodeblock %}

