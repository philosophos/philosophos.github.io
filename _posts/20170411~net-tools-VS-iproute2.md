---
layout: post
title: net-tools VS iproute2
copyright: true
date: 2017-04-11 19:13:25
updated:
description:
categories:
- Linux
- Network
tags:
- Linux
- Network
- net-tools
- iproute2
- ifconfig
---

Show All Connected Network Interfaces
Show IPv4 Address(es) of a Network Interface
Show IPv6 address(es) of a Network Interface
Activate or Deactivate a Network Interface
Assign IPv4 address(es) to a Network Interface
Remove an IPv4 address from a Network Interface
Assign an IPv6 address to a Network Interface
Remove an IPv6 address from a Network Interface
Change the MAC Address of a Network Interface
View the IP Routing Table
Add or Modify a Default Route
Add or Remove a Static Route
View Socket Statistics
View the ARP Table
Add or Remove a Static ARP Entry
Add, Remove or View Multicast Addresses
<!-- more -->

-----------------------------------------------------------
| net-tools | iproute2 |
| - | - |
| ifconfig -a             | ip link show               |
| ifconfig eth1           | ip addr show dev eth1      |
| ifconfig eth1           | ip -6 addr show dev eth1   |
| sudo ifconfig eth1 up   | sudo ip link set up eth1   |
| sudo ifconfig eth1 down | sudo ip link set down eth1 |
| sudo ifconfig eth1 10.0.0.1/24                      | sudo ip addr add 10.0.0.1/24 dev eth1               |
| sudo ifconfig eth1 0                                | sudo ip addr del 10.0.0.1/24 dev eth1               |
| sudo ifconfig eth1 inet6 add 2002:0db5:0:f102::1/64 | sudo ip -6 addr add 2002:0db5:0:f102::1/64 dev eth1 |
| sudo ifconfig eth1 inet6 del 2002:0db5:0:f102::1/64 | sudo ip -6 addr del 2002:0db5:0:f102::1/64 dev eth1 |
| sudo ifconfig eth1 hw ether 08:00:27:75:2a:66       | sudo ip link set dev eth1 address 08:00:27:75:2a:67 |
| route -n <br>netstat -rn                                   | ip route show                                             |
| sudo route add default gw 192.168.1.2 eth0                 | sudo route del default gw 192.168.1.1 eth0                |
| sudo ip route add default via 192.168.1.2 dev eth0         | sudo ip route replace default via 192.168.1.2 dev eth0    |
| sudo route add -net 172.16.32.0/24 gw 192.168.1.1 dev eth0 | sudo ip route add 172.16.32.0/24 via 192.168.1.1 dev eth0 |
| sudo route del -net 172.16.32.0/24                         | sudo ip route del 172.16.32.0/24                          |
| netstat <br> netstat -l                                    | ss <br> ss -l                                             |
| arp -an                                     | ip neigh                                                          |
| sudo arp -s 192.168.1.100 00:0c:29:c0:5a:ef | sudo ip neigh add 192.168.1.100 lladdr 00:0c:29:c0:5a:ef dev eth0 |
| sudo arp -d 192.168.1.100                   | sudo ip neigh del 192.168.1.100 dev eth0                          |
| sudo ipmaddr add 33:44:00:00:00:01 dev eth0 | sudo ip maddr add 33:44:00:00:00:01 dev eth0                      |
| sudo ipmaddr del 33:44:00:00:00:01 dev eth0 | sudo ip maddr del 33:44:00:00:00:01 dev eth0                      |
| ipmaddr show dev eth0 <br>netstat -g        | ip maddr list dev eth0                                            |

