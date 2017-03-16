---
layout: post
title: Services & Ports
copyright: true
date: 2017-04-09 02:07:19
updated:
description:
categories:
- Network
tags:
- Network
- Service
- Port
---

port:1~65535
知名端口号:1~1023
<!-- more -->

|                     |                                     |            | port                                 |
| -                   | -                                   | -          | :-                                   |
| amanda-client       |                                     | TCP/UDP    | 10080
| bacula              |                                     | TCP        | 9101-9103
| bacula-client       |                                     | TCP        | 9102
| BGP                 | Border Gateway Protocol             | TCP        | 179
| BootP               | Bootstrap Protocol                  |            | 67
| DHCP                | Dynamic Host Configuration Protocol | UDP        | 67
| dhcpv6              |                                     | UDP        | 547
| dhcpv6-client       |                                     | UDP        | 546
| DNS                 | Domain name System                  | UDP        | 53
| FTP                 | File Transfer Protocol              | TCP        | 20:data 21:link
| HTTP                | Hypertext Transfer Protocol         | TCP        | 80
| HTTPS               | Hypertext Transfer Protocol Secure  | TCP        | 443
| IMAP4               | Internet Message Access Protocol    |            | 143
| IMAPs               |                                     | TCP        | 993
| IPP                 |                                     | TCP/UDP    | 631
| IPP-client          |                                     | UDP        | 631
| IPsec               |                                     | ah,esp,UDP | 500
| ISCSI-target        |                                     | TCP/UDP    | 3260
| kerberos            |                                     | TCP/UDP    | 88
| kpasswd             |                                     | TCP/UDP    | 464
| ldap                |                                     | TCP        | 389
| ldaps               |                                     | TCP        | 636
| libvirt             |                                     | TCP        | 16509
| libvirt-tls         |                                     | TCP        | 16509
| mdns                |                                     | UDP        | 5353
| mountd              |                                     | TCP/UDP    | 20048
| ms-wbt              |                                     | TCP        | 3389
| mysql               |                                     | TCP        | 3306
| NFS                 | Network File System                 | TCP        | 2049
| NTP                 | Network News Transfer Protocol      |            | 123
| openVPN             | open Virtual Private Network        | UDP        | 1194
| pmcd                |                                     | TCP        | 44321
| pmproxy             |                                     | TCP        | 44322
| pmwebapi            |                                     | TCP        | 44323
| pmwebapis           |                                     | TCP        | 44324
| pop3s               |                                     | TCP        | 995
| POP3                | Post Office Protocol                | UDP        | 110
| postgreSQL          |                                     | TCP        | 5432
| proxy-dhcp          |                                     | UDP        | 4011
| radius              |                                     | UDP        | 1812,1813
| RH-satellite-6      |                                     | TCP        | 80,443,5646,5647,5671,8140,8080,9090
| RIP                 | Routing Information Protocol        | UDP        | 520
| RPC-bind            |                                     | TCP/UDP    | 111
| rsyncd              |                                     | TCP/UDP    | 873
| samba               |                                     | TCP/UDP    | 137,138 / 139,445
| samba-client        |                                     | UDP        | 137,138
| SMTP                | Simple Mail Transfer Protocol       | TCP        | 25
| SNMP3               | Simple Network Management Protocol  | UDP/TCP    | 161,162
| SSH                 | Secure Shell                        | TCP        | 22
| Telnet              | Terminal emulation Protocol         | TCP        | 23
| TFTP                |                                     | UDP        | 69
| TFTP-client         |
| transmission-client |                                     | TCP        | 51413
| vdsm                |                                     | TCP        | 54321,5900-6923,49152-49216
| vnc-server          |                                     | TCP        | 5900-5903
| wbem-https          |                                     | TCP        | 5989

