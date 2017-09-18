---
layout: post
title: Manage Wireless Network Manually
copyright: true
date: 2017-04-10 20:01:08
updated:
description:
categories:
- Linux
- Network
tags:
- Linux
- Network
- Wireless
- Manually
---

Rfkill caveat
`iwconfig` & `iw`
Association with WPA
Assigning IP address
<!-- more -->

-----------------------------
**Rfkill caveat**

    rfkill unbound wlan

-----------------------------
`iwconfig` & `iw`
-----------------------------
<table><tr><th>iwconfig</th><th>iw</th></tr>
<tr><td>iwconfig wlan0 txpower on                       </td><td>                                                      </td></tr> <tr><td>iwconfig wlan0 txpower auto                     </td><td>iw dev wlan0 set txpower auto                         </td></tr> <tr><td>iwconfig wlan0 txpower fixed                    </td><td>iw dev wlan0 set txpower fixed 50                     </td></tr> <tr><td>iwconfig wlan0 txpower 1  #100dBm               </td><td>iw dev wlan0 set txpower limited 50                   </td></tr> <tr><td>iwconfig wlan0 rts 512    #0~65535B             </td><td>iw phy phy0 set rts 512   #0~65535B                   </td></tr> <tr><td>iwconfig wlan0 rts auto                         </td><td>                                                      </td></tr> <tr><td>iwconfig wlan0 frag 512   #256~2350B            </td><td>iw phy phy0 set frag 512  #256~2348B                  </td></tr> <tr><td>iwconfig wlan0 frag auto                        </td><td>                                                      </td></tr> <tr><td>iwconfig wlan0 / ifconfig wlan0                 </td><td>iw dev / iw dev wlan0 link / iw dev wlan0 station dump</td></tr> <tr><td>ifconfig wlan0 up                               </td><td>ip link set wlan0 up                                  </td></tr> <tr><td>ifconfig wlan0                                  </td><td>ip link show wlan0                                    </td></tr> <tr><td>iwconfig wlan0 mode ad-hoc                      </td><td>iw dev wlan0 set type ibss                            </td></tr> <tr><td>iwlist wlan0 scan| grep SSID                    </td><td>iw dev wlan0 scan| grep SSID                          </td></tr> <tr><td>iwconfig wlan0 essid [the-essid]  #no-encryption</td><td>iw dev wlan0 connect [the-essid]  #no-encryption      </td></tr> <tr><td>iwconfig wlan0 essid [the-essid] key s:[WEP-key]</td><td>iw dev wlan0 connect [the-essid] key 0:[WEP-key]      </td></tr>
</table>

-----------------------------
Association with WPA
-----------------------------

    wpa_passphrase asdfdsaqaz blblxmxa > /etc/wpa_supplicant/dingzhenglin.conf
    wpa_supplicant -B -iwlan0 -c /etc/wpa_supplicant/dingzhenglin.conf
or

    wpa_supplicant -B -iwlan0 -c < (wpa_passphrase asdfdsaqaz blblxmxa)

-----------------------------
Assigning IP address
-----------------------------

    dhcpcd wlan0
