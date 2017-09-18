---
layout: post
title: Arch Linux Installation Record
date: 2017-02-17 20:54:43
updated: 2017-02-18 22:52:23
copyright: true
description:  Arch | Linux | Install
categories: Linux
tags: 
- Arch 
- Linux 
- Install
---

- Temporary Configuration 临时设置
- Net-link & Source Configuration 联网&设置软件源
- Create Partition 分区
- MKFS 创建文件系统
- MOUNT 挂载目标分区
- BASE Installation 安装基础系统
- Base Configuration 配置基础系统
- Boot Configuration 启动引导配置
- Xorg & GPU Driver
- Input Method：fcitx/ibus/rime
- CLI Program
- GUI Program
- More Configuration
<!-- more -->

-------------------------------------------------------------------------------
Temporary Configuration 临时设置
-------------------------------------------------------------------------------
如果可以暂时忍受tty的默认字体，习惯QWER键盘，看英文不太困难，忽略这一步
**键盘布局：**
`localectl list-keymaps`		# 查看可用键盘布局
`loadkeys <键盘布局>`
**终端字体：**
`ls /usr/share/kbd/consolefonts/`		# 查看可用终端字体
`setfont <终端字体>`
**显示语言：**
`vi /etc/locale.gen`		# 反注释需要的 locale
`locale-gen`
`export LANG=en_US.UTF-8`

-------------------------------------------------------------------------------
Net-link & Source Configuration 联网&设置软件源
-------------------------------------------------------------------------------

`pppoe-setup`
`systemctl start adsl`
`timedatectl set-ntp true`

更新本地数据库,可选
`pacman -Syy`

简单粗暴的做法：选择所有的中国大陆的源
`sed -i '/Score/{/China/!{n;s/^/#/}}' /etc/pacman.d/mirrorlist`
教育网用户：选浙大，清华，中科大的源
`sed -i '/zju\|tuna\|ustc/!s/^Server/#Server/' /etc/pacman.d/mirrorlist`
本地有镜像站就用本地的

检查结果
`grep -A1 China /etc/pacman.d/mirrorlist`

添加archlinuxcn源
{% codeblock lang:sh Add the following to /etc/pacman.conf %}
    [archlinuxcn]
    #Server=http://mirrors.xdlinux.info/archlinuxcn/$arch
    Server=https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
    Server=https://mirrors.ustc.edu.cn/archlinuxcn/$arch
{% endcodeblock %}

    pacman -S archlinuxcn-keyring

-------------------------------------------------------------------------------
Create Partition 分区
-------------------------------------------------------------------------------
SSD请参考
[SSD（固态硬盘）简介](http://www.jinbuguo.com/storage/ssd_intro.html)
[Linux环境SSD（固态硬盘）使用指南](http://www.jinbuguo.com/storage/ssd_usage.html)
[archlinux wiki:SSD](https://wiki.archlinux.org/index.php/Solid_State_Drive)

**分区用软件**
通用：parted
MBR:fdisk,cfdisk,sfdisk
GPT:gdisk,cgdisk,sgdisk
注：
在debian安装分区工具parted中，1KB=1000B，1MB=1000KB，1GB=1000MB
在fdisk/gdisk,cfdisk/cgdisk & ls,lsblk中，1KB=1024B，1MB=1024KB，1GB=1024MB;

手动安装推荐cfdisk/cgdisk，伪图形化界面，直观形象
`cgdisk /dev/sda`

-------------------------------------------------------------------------------
MKFS 创建文件系统
-------------------------------------------------------------------------------
文件系统个人用户没特殊需求就选EXT4
加上惰性初始化参数`-E lazy_itable_init=0,lazy_journal_init=0`，格式化会慢些，以后使用会稍快

    mkfs.fat -F32 /dev/sda6
    mkswap /dev/mapper/arch-5.swap
    swapon /dev/mapper/arch-5.swap
    mkfs.ext4 -c -b 4096 -E lazy_itable_init=0,lazy_journal_init=0 /dev/sda7
    time mkfs.ext4 -c -b 4096 -E lazy_itable_init=0,lazy_journal_init=0 /dev/mapper/arch-1.root
    time mkfs.ext4 -c -b 4096 -E lazy_itable_init=0,lazy_journal_init=0 /dev/mapper/arch-2.usr
    time mkfs.ext4 -c -b 4096 -E lazy_itable_init=0,lazy_journal_init=0 /dev/mapper/arch-3.var

-------------------------------------------------------------------------------
MOUNT 挂载目标分区
-------------------------------------------------------------------------------

    mount /dev/mapper/arch-1.root /mnt
    mkdir /mnt/{boot,usr,var}
    mount /dev/sda7 /mnt/boot
    mkdir -p /mnt/boot/EFI
    mount /dev/sda6 /mnt/boot/EFI
    mount /dev/mapper/arch-2.usr /mnt/usr
    mount /dev/mapper/arch-3.var /mnt/var

-------------------------------------------------------------------------------
Base Installation 安装基础系统
-------------------------------------------------------------------------------

    pacstrap -i /mnt base base-devel

    pacstrap -i /mnt \
    linux-lts linux-lts-headers \
    linux-zen linux-zen-headers \
    linux-grsec linux-grsec-headers \
    net-tools networkmanager rp-pppoe rfkill bluez bluez-utils ifplugd \
    iw wpa_actiond wpa_supplicant wireless_tools dialog broadcom-wl-dkms \
    cpupower turbostat usbip lm_sensors alsa-utils alsa-plugins \
    iftop iotop htop atop ntop sysstat dstat vnstat lsof multitail tcpdump \
    dosfstools grub efibootmgr syslinux \
    parted gptfdisk fuse exfat-utils ntfs-3g zip unzip unrar p7zip \
    udisks2 udiskie udevil nfs-utils sshfs curlftpfs cifs-utils zenity \
    libmtp android-file-transfer archlinuxcn/simple-mtpfs ranger vifm \
    python-chardet libcaca highlight atool poppler transmission-cli mediainfo perl-image-exiftool \
    bash-completion bash-docs bashdb \
    zsh-completions zsh-doc zshdb zsh-lovers zsh-syntax-highlighting archlinuxcn/oh-my-zsh-git \
    fbterm fbgrab fbv tmux w3m vim gvim neovim ctags archlinuxcn/vim-youcompleteme-git \
    tree pv most lesspipe cdu pydf lolcat colordiff prettyping figlet archlinuxcn/toilet \
    minicom picocom tinyserial gpm bc calc dos2unix ack mlocate man-pages-zh_cn \
    dictd sdcv ydcv axel aria2 wget git mercurial subversion \
    firewalld ntp hostapd dhcp bind bind-tools dnsmasq pdnsd dnscrypt-proxy dnssec-tools \
    openssh sshfs sshguard rsync zsync inotify-tools parallel proxychains shadowsocks \
    cmake ccache distcc colorgcc $(pacman -Ssq noto-fonts) \
    emacs-nox emacs pandoc pandoc-citeproc texlive-core texlive-lang texlive-most \
    python2 python python2-pip python-pip lua luarocks nodejs npm ruby \
    jre8-openjdk jre8-openjdk-headless jdk8-openjdk openjdk8-doc jdk \
    mariadb perl-dbd-mysql mytop innotop phpmyadmin \
    postgresql postgresql-docs phppgadmin python2-mysql2pgsql \
    mongodb mongodb-tools redis \
    i3 awesome vicious xorg-server-xephyr xorg-xev xorg-xprop xcompmgr transset-df conky \
    xsel xclip archlinuxcn/xscreensaver-arch-logo xterm rxvt-unicode urxvt-perls \
    x11vnc tigervnc jfbview apvlv feh scrot vlc  firefox archlinuxcn/google-chrome \
    wps-office netease-cloud-music \
    archlinuxcn/anything-sync-daemon archlinuxcn/profile-sync-daemon 

**生成配置fstab**
`genfstab -p /mnt > /mnt/etc/fstab && vi /mnt/etc/fstab`

-------------------------------------------------------------------------------
Base Configuration 配置基础系统
-------------------------------------------------------------------------------
### Language&Date&Hostname 语言&时间&主机名

    arch-chroot /mnt /bin/bash
    sed -i '/de_\|el_\|en_\|es_\|fr_\|ja_\|ko_\|pt_\|ru_\|zh_/s/^#//'  /etc/locale.gen
    locale-gen && echo LANG=en_US.UTF-8  > /etc/locale.conf
    ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
    hwclock --systohc --utc
    echo mobius-8 > /etc/hostname

### Base Network Configuration 基础服务网络配置

    systemctl enable lvm2-lvmetad
    systemctl start firewalld 
    systemctl enable firewalld 
    systemctl enable dhcpcd
    systemctl enable NetworkManager
    pacman -S --needed iw wpa_supplicant dialog
    pacman -S --needed rp-pppoe
    pppoe-setup
    systemctl enable adsl


### archlinuxcn & yaourt 添加源
{% codeblock lang:sh Add the following to /etc/pacman.conf %}
    [archlinuxcn]
    #Server=http://mirrors.xdlinux.info/archlinuxcn/$arch
    Server=https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
    Server=https://mirrors.ustc.edu.cn/archlinuxcn/$arch
{% endcodeblock %}
{% codeblock lang:sh %}
    pacman -S archlinuxcn-keyring yaourt
{% endcodeblock %}
{% codeblock lang:sh Edit /etc/yaourtrc %}
AURURL="https://aur.tuna.tsinghua.edu.cn"
{% endcodeblock %}

### Base Sec Configuration 基础安全配置

    sed -i '/^tty/s/^/#/'/etc/securetty
    sed -i '/PermitRootLogin/s/prohibit-password/no#prohibit-password/' /etc/ssh/sshd_config
    sed -i '/PermitRootLogin/s/yes/no/' /etc/ssh/sshd_config
    sed -i '/^#PermitRootLogin/s/#//' /etc/ssh/sshd_config
    sed -i '/Port/s/22/2222/' /etc/ssh/sshd_config
    
### Mouse Configuration 鼠标配置

{% codeblock lang:sh Add /etc/conf.d/gpm %}
    #GPM_ARGS="-m /dev/psaux" -t ps2	#PS/2 mouse
     GPM_ARGS="-m /dev/input/mice" -t imps2	#USB mouse
    #GPM_ARGS="-m /dev/input/mice" -t ps2	#IBM Trackpoints
{% endcodeblock %}

### tmux-mem-cpu-load (for tmux)

    git clone https://github.com/thewtex/tmux-mem-cpu-load.git /tmp
    cd /tmp/tmux-mem-cpu-load
    cmake .
    make
    su -c 'make install'

### Other Configuration

    mkdir -p /media/{usual,ISO,mtp}
    cp -rf /media/usual/bak/conf.unix/skel/ /etc/
    useradd -u 1536 -m -o -s /bin/zsh jeau-0
    passwd jeau-0
    passwd
    chown -R jeau-0:jeau-0 /media/jeau-0/
    gpasswd -a jeau-0 video
    gpasswd -a jeau-0 uucp
    setcap 'cap_sys_tty_config+ep' /usr/bin/fbterm
    mkdir /usr/share/stardict/dic/
    ls stardict-* |xargs -i tar xjvf {} -C /usr/share/stardict/dic/

-------------------------------------------------------------------------------
Boot Configuration 启动引导配置
-------------------------------------------------------------------------------
### mkinitcpio
{% codeblock lang:sh Edit /etc/mkinitcpio.conf %}
HOOKS="base udev autodetect modconf block lvm2 resume filesystems keyboard shutdown usr fsck"
{% endcodeblock %}
`lvm2` for LVM
`resume` for Hibernate
`usr` for `/usr` mount independently

    mkinitcpio -p linux

### grub installation
BIOS：

    pacman -S --needed grub os-prober
    grub-install --recheck /dev/<目标磁盘>

UEFI：

    pacman -S --needed dosfstools grub efibootmgr  os-prober
    grub-install --target=x86_64-efi --efi-directory=/boot/EFI \
    --bootloader-id=archlinux --recheck

### grub config

{% codeblock lang:sh Edit /etc/default/grub %}
# GRUB can remember the last entry you booted from and use this as the default entry to boot from next time.
# This is useful if you have multiple kernels or operating systems
GRUB_DEFAULT=saved
GRUB_SAVEDEFAULT="true"
# If you have multiple kernels installed,by default grub-mkconfig groups them in a submenu.
GRUB_DISABLE_SUBMENU=y

GRUB_CMDLINE_LINUX_DEFAULT="resume=/dev/mapper/arch-5.swap quiet"
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"

# Uncomment to enable Hidden Menu, and optionally hide the timeout count
GRUB_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT=3
GRUB_HIDDEN_TIMEOUT_QUIET=true

# Uncomment and set to the desired menu colors.  Used by normal and wallpaper
# modes only.  Entries specified as foreground/background.
GRUB_COLOR_NORMAL="light-blue/black"
GRUB_COLOR_HIGHLIGHT="light-cyan/blue"

# Uncomment to allow the kernel use the same resolution used by grub
GRUB_GFXPAYLOAD_LINUX=keep
{% endcodeblock %}

`resume=/dev/mapper/arch-5.swap` specifies swap partition for hibernation
`net.ifnames=0 biosdevname=0` retain tradtional NIC names like 'eth0' instead of 'enp0s3'

    grub-mkconfig -o /boot/grub/grub.cfg

**exit**

C-d
`umount -R /mnt`
`reboot`

-------------------------------------------------------------------------------
Xorg & GPU Driver
-------------------------------------------------------------------------------
`lspci | egrep -i 'nvidia|amd|geforce|vga|3d|graphic'`

	# # graphics driver in offical mirrors：
    # # +----------------------+--------------------+--------------+
    # # |                      |        开源         |     私有     |
    # # +----------------------+--------------------+--------------+
    # # |         通用          |   xf86-video-vesa  |              |
    # # +----------------------+--------------------+--------------+
    # # |         Intel        |  xf86-video-intel  |              |
    # # +--------+-------------+--------------------+--------------+
    # # |        | GeForce 7+  |                    |    nvidia    |
    # # | nVidia +-------------+ xf86-video-nouveau +--------------+
    # # |        | GeForce 6/7 |                    | nvidia-304xx |
    # # +--------+-------------+--------------------+--------------+
    # # |        AMD/ATI       |   xf86-video-ati   |              |
    # # +----------------------+--------------------+--------------+
  

    pacman -S --needed \
    xorg-server \
    xorg-server-utils \
    xorg-xinit \
    xf86-input-synaptics \
    xf86-input-evdev \
    xf86-video-intel \

    pacman -S --needed \
    nvidia-dkms \
    bbswitch primus bumblebee \

    pacman -S --needed \
    nvidia-settings nvidia-utils nvdock \


`pacman -S --needed cuda opencl-nvidia `

-------------------------------------------------------------------------------
Input Method：fcitx/ibus/rime
-------------------------------------------------------------------------------

    pacman -S --needed \
    fcitx \
    fcitx-configtool \
    fcitx-fbterm \
    fcitx-gtk2 \
    fcitx-gtk3 \
    fcitx-table-extra \
    fcitx-table-other \
    fcitx-ui-light \
    fcitx-cloudpinyin \
    fcitx-sunpinyin \
    fcitx-m17n \
    archlinuxcn/fcitx-sogoupinyin

-------------------------------------------------------------------------------
kernel
-------------------------------------------------------------------------------

    linuxdoc-tools linux-manpages kexec-tools \
    linux-lts linux-lts-headers \
    linux-zen linux-zen-headers \
    linux-grsec linux-grsec-headers \

-------------------------------------------------------------------------------
CLI Program
-------------------------------------------------------------------------------
### Network
**net-config** : iproute2-doc net-tools rfkill ifplugd networkmanager
**PPPOE**      : rp-pppoe
**Bluetooth**  : bluez bluez-utils
**wireless**   : iw wpa_actiond wpa_supplicant wireless_tools dialog
**AP**         : hostapd
**NTP**        : ntp
**DHCP**       : dhcp
**DNS-Server** : bind bind-tools
**DNS-Cache**  : dnsmasq pdnsd
**DNS-Sec**    : dnscrypt-proxy dnssec-tools
**SSH**        : openssh sshfs sshguard aur/pssh
**Proxy**      : proxychains shadowsocks goagent
**Firewall**   : iptables firewalld
**Download**   : axel aria2 wget

### System Tools
**hardware control**  : cpupower turbostat usbip
**Sleep&Hibernate**   : aur/pm-utils
**Sensors**           : lm_sensors
**Volume**            : alsa-utils alsa-plugins
**System Monitoring** : iftop iotop htop atop ntop sysstat dstat vnstat lsof multitail tcpdump time
**BOOT**              : dosfstools grub efibootmgr syslinux
**fonts**             : $(pacman -Ssq noto-fonts)

### File&Storage&Disk
**PART**                           : parted gptfdisk
**FS**                             : fuse exfat-utils ntfs-3g
**Udisk**                          : udisks2 udiskie udevil
**NFS**                            : nfs-utils
**Samba**                          : cifs-utils
**SSHFS**                          : sshfs
**FTP**                            : curlftpfs
**Sync**                           : rsync zsync inotify-tools archlinuxcn/anything-sync-daemon archlinuxcn/profile-sync-daemon
**MTP**                            : libmtp archlinuxcn/simple-mtpfs
**File Manager**                   : ranger
**PDF**                            : ghostscript archlinuxcn/pdftk-bin
**achiving & compression**         : zip unzip unrar p7zip

### Shell&Terminal
**Bash**                 : bash-completion bash-docs bashdb
**Zsh**                  : zsh-completions zsh-doc zshdb zsh-lovers zsh-syntax-highlighting
**Regex**                : ack sed gawk
**File Finder**          : mlocate
**Fbterm**               : fbterm fbgrab fbv
**Terminal**             : tmux parallel
**Serial Communication** : minicom picocom tinyserial
**VIM**                  : vim gvim neovim ctags vim-youcompleteme-git
**EMACS**                : emacs-nox emacs pandoc pandoc-citeproc
**view/color**           : tree pv most lesspipe cdu pydf lolcat colordiff prettyping
**code highlighter**     : highlight
**Dict**                 : dictd sdcv ydcv
**Calc**                 : bc calc
**TTY REC**              : aur/ttyrec aur/termrec aur/ippt aur/tpp
**WordArt**              : figlet toilet
**other tools**          : gpm dos2unix man-pages-zh_cn

### Code&Language&Package
**compile** : clang cmake ccache distcc colorgcc
**VCS**     : git hub tig mercurial subversion
**texlive** : texlive-core texlive-lang texlive-most
**python2** : python2 python2-pip
**python3** : python python-pip
**nodejs**  : nodejs npm | cnpm hexo-cli
**ruby**    : ruby ruby-jekyll
**lua**     : lua luarocks
**JDK**     : jre8-openjdk jre8-openjdk-headless jdk8-openjdk openjdk8-doc jdk aur/jdk-docs

### Virtual
**docker**             : docker docker-compose docker-machine
**virt-manage**        : libvirt virt-install
**net-manage**         : bridge-utils openvswitch vlan quagga
**qemu**               : qemu \
qemu-arch-extra \
qemu-block-gluster \
qemu-block-iscsi \
qemu-block-rbd \

-------------------------------------------------------------------------------
GUI Program
-------------------------------------------------------------------------------
**Display manager**: sddm
**Window Manager** : i3 awesome vicious
**X Tools**        : xorg-server-xephyr xorg-xev xorg-xprop
**Transparency**   : xcompmgr transset-df
**Backlight**      : xorg-xbacklight archlinuxcn/light
**Monitor**        : conky
**clipboard**      : xsel xclip
**screensaver**    : archlinuxcn/xscreensaver-arch-logo
**terminal**       : xterm rxvt-unicode urxvt-perls
**Browser**        : firefox archlinuxcn/google-chrome
**VNC**            : x11vnc tigervnc
**PDF**            : zathura zathura-cb zathura-djvu zathura-pdf-mupdf zathura-ps archlinuxcn/masterpdfeditor
**PHOTO**          : feh sxiv geeqie shotwell
**Raster Graphics Editors**: imagemagick imagemagick-doc gimp gimp-help-en gimp-help-zh_cn archlinuxcn/gimp-plugin-resynthesizer-git
**Vector Graphics Editors**: graphviz dot2tex xdot umbrello umlet plantuml dia sk1 inkscape python2-scour uniconvertor
**Screenshot**     : scrot
**VIDEO**          : vlc
**OFFICE**         : archlinuxcn/wps-office
**MUSIC**          : archlinuxcn/netease-cloud-music
**Note**           : aur/[turtl](https://turtl.it) aur/[simplenote-electron-bin](https://github.com/Automattic/simplenote-electron) aur/[laverna](https://laverna.cc/)

-------------------------------------------------------------------------------
More Configuration
-------------------------------------------------------------------------------
[VIM](https://github.com/philosophos/easy-vimrc)         -- the God of Text Editor,For Human
[TMUX](https://github.com/philosophos/tmux.conf)         -- Terminal Multiplexer,For Efficiency
[ZSH](https://github.com/philosophos/power-zshrc)        -- The last shell you’ll ever need
[URxvt&XTerm](https://github.com/philosophos/Xresources) -- Classic Terminal,Lesser Dependencies
[Awesome](https://github.com/philosophos/awesome-WM-rc)  -- Tilling Window Manager,Highly Configuration
[Conky](https://github.com/philosophos/conky.conf)       -- Lightweight System Monitor for X，Highly Configuration
