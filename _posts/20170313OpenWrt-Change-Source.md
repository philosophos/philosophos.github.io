---
layout: post
title: OpenWrt -- Change Source with HTTPS
copyright: true
date: 2017-03-13 22:21:47
updated:
description: Change OpenWrt Source with HTTPS
Categories:
- Linux
- OpenWrt
tags:
- Linux
- OpenWrt
- source
- HTTPS
---

`vim scp://root@OpenWrt:2222//etc/opkg/distfeeds.conf`
`opkg update` Failed.
Maybe the wget in BusyBox doesn't support https.
Change source with HTTP, update,
Install `wget` & `ca-certificates`,
Change source with HTTPS, update.
<!-- more -->

-----------------------------------------------------------

    root@OpenWrt:~# opkg update
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_base.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.sig.
    Signature check passed.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_luci.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.sig.
    Signature check passed.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_packages.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.sig.
    Signature check passed.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_routing.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.sig.
    Signature check passed.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_telephony.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.sig.
    Signature check passed.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_management.
    Downloading http://downloads.openwrt.org/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.sig.
    Signature check passed.

    root@OpenWrt:~# type vim
    vim is an alias for vi
    root@OpenWrt:~# type vi
    vi is /bin/vi
Oh, I can't stand vi. So Edit the file in my PC,
{% codeblock lang:sh $ vim scp://root@192.168.1.1:2222//etc/opkg/distfeeds.conf %}
src/gz chaos_calmer_base https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base
src/gz chaos_calmer_luci https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci
src/gz chaos_calmer_packages https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages
src/gz chaos_calmer_routing https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing
src/gz chaos_calmer_telephony https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony
src/gz chaos_calmer_management https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management
{% endcodeblock %}

    root@OpenWrt:~# opkg update
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.gz.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.sig.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Signature check failed.
    Remove wrong Signature file.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.gz.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.sig.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Signature check failed.
    Remove wrong Signature file.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.gz.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.sig.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Signature check failed.
    Remove wrong Signature file.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.gz.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.sig.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Signature check failed.
    Remove wrong Signature file.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.gz.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.sig.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Signature check failed.
    Remove wrong Signature file.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.gz.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.sig.
    wget: can't execute 'openssl': No such file or directory
    wget: error getting response: Connection reset by peer
    Signature check failed.
    Remove wrong Signature file.
    Collected errors:
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.gz, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.sig, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.gz, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.sig, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.gz, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.sig, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.gz, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.sig, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.gz, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.sig, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.gz, wget returned 1.
     * opkg_download: Failed to download https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.sig, wget returned 1.

Maybe the wget in BusyBox doesn't support https.
Change source with HTTP, update, install `wget` & `ca-certificates`, then change source with HTTPS, update .

    root@OpenWrt:~# sed -i 's/https/http/' /etc/opkg/distfeeds.conf

    root@OpenWrt:~# opkg update
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_base.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.sig.
    Signature check passed.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_luci.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.sig.
    Signature check passed.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_packages.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.sig.
    Signature check passed.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_routing.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.sig.
    Signature check passed.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_telephony.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.sig.
    Signature check passed.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_management.
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.sig.
    Signature check passed.

    root@OpenWrt:~# opkg install wget ca-certificates
    Installing wget (1.17.1-1) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/wget_1.17.1-1_mvebu.ipk.
    Installing libpcre (8.38-1) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/libpcre_8.38-1_mvebu.ipk.
    Installing libopenssl (1.0.2g-1) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/libopenssl_1.0.2g-1_mvebu.ipk.
    Installing zlib (1.2.8-1) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/zlib_1.2.8-1_mvebu.ipk.
    Installing librt (0.9.33.2-1) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/librt_0.9.33.2-1_mvebu.ipk.
    Installing libpthread (0.9.33.2-1) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/libpthread_0.9.33.2-1_mvebu.ipk.
    Installing ca-certificates (20150426) to root...
    Downloading http://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/ca-certificates_20150426_mvebu.ipk.
    Configuring libpcre.
    Configuring zlib.
    Configuring libopenssl.
    Configuring libpthread.
    Configuring librt.
    Configuring wget.
    Configuring ca-certificates.

    root@OpenWrt:~# sed -i 's/http/https/' /etc/opkg/distfeeds.conf

    root@WRT:~# opkg update
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_base.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/base/Packages.sig.
    Signature check passed.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_luci.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/luci/Packages.sig.
    Signature check passed.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_packages.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/packages/Packages.sig.
    Signature check passed.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_routing.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/routing/Packages.sig.
    Signature check passed.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_telephony.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/telephony/Packages.sig.
    Signature check passed.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.gz.
    Updated list of available packages in /var/opkg-lists/chaos_calmer_management.
    Downloading https://mirrors.tuna.tsinghua.edu.cn/openwrt/chaos_calmer/15.05.1/mvebu/generic/packages/management/Packages.sig.
    Signature check passed.
