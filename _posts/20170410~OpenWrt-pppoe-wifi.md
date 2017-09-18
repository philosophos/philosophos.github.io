---
layout: post
title: OpenWrt -- pppoe & wifi
copyright: true
date: 2017-04-10 18:11:28
updated:
description:
categories:
- Linux
- OpenWrt
tags:
- Linux
- OpenWrt
- Router
- Network
- PPPOE
- wifi
---

- setup pppoe
- setup wifi
<!-- more -->

-----------------------------
setup pppoe
-----------------------------

    uci set network.wan.proto=pppoe
    uci set network.wan.username='yougotthisfromyour@isp.su'
    uci set network.wan.password='yourpassword'
    uci commit network
    ifup wan

-----------------------------
setup wifi
-----------------------------
 
    uci show wireless

    uci set wireless.radio0.disabled='0'
    uci set wireless.radio1.disabled='0'
    uci set wireless.@wifi-iface[0].ssid='SYS-H'
    uci set wireless.@wifi-iface[1].ssid='SYS-L'
    uci set wireless.@wifi-iface[0].encryption=psk2
    uci set wireless.@wifi-iface[1].encryption=psk2
    uci set wireless.@wifi-iface[0].key='password0'
    uci set wireless.@wifi-iface[1].key='password1'
    uci commit wireless
    wifi

