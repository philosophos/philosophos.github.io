---
layout: post
title: OSPFv2 LSA Message Format
copyright: true
date: 2017-09-23 20:24:52
updated:
description:
categories:
- Network
tags:
- Network
- PDU
- Routing
- OSPF
- OSPFv2
- LSA
---

OSPFv2 Protocol LSA Header
- OSPFv2 Router LSA
- OSPFv2 Network LSA
- OSPFv2 Network/ASBR Summary LSA
- OSPFv2 Autonomous System External LSA
- OSPFv2 Not-So-Stubby Area External LSA

<!-- more -->

-----------------------------------------------------------
OSPFv2 Protocol LSA Header
-----------------------------------------------------------

    ┃ 16bit    |  8bit  | 8bit ┃
    ┏----------+--------+------┓
    ┃ Age      | Option | Type ┃
    ┠----------+--------+------┨
    ┃ Link State ID            ┃
    ┠--------------------------┨
    ┃ Advertising Router       ┃
    ┠--------------------------┨
    ┃ Sequence                 ┃
    ┠----------+---------------┨
    ┃ Checksum | Length        ┃
    ┗----------+---------------┛

| Type | LSA Type                       | Command                             |
|------|--------------------------------|-------------------------------------|
|    1 | Router LSA                     | show ip ospf database router        |
|    2 | Network LSA                    | show ip ospf database network       |
|    3 | Network Summary LSA            | show ip ospf database summary       |
|    4 | ASBR Summary LSA               | show ip ospf database asbr-summary  |
|    5 | AS External LSA                | show ip ospf database external      |
|    6 | Group Membership LSA           |                                     |
|    7 | NSSA External LSA              | show ip ospf database nssa-external |
|    8 | External Attributes LSA        | show ip ospf database               |
|    9 | Opaque LSA (link链路本地范围 ) |                                     |
|   10 | Opaque LSA (area本地区域范围)  |                                     |
|   11 | Opaque LSA (AS范围)            |                                     |

#### OSPFv2 Router LSA

    ┃ 8bit      | 8bit          | 8bit   | 8bit   ┃
    ┏---------------------------+--------+--------┓
    ┃ Age                       | Option | Type=1 ┃
    ┠---------------------------+--------+--------┨
    ┃ Link State ID                               ┃
    ┠---------------------------------------------┨
    ┃ Advertising Router                          ┃
    ┠---------------------------------------------┨
    ┃ Sequence                                    ┃
    ┠---------------------------+-----------------┨
    ┃ Checksum                  | Length          ┃
    ┠-----------+---------------+-----------------┨
    ┃00000|V|E|B| 0x00          | Number of Links ┃
    ┠-----------+---------------+-----------------┨
    ┃ Link ID                                     ┃
    ┠---------------------------------------------┨
    ┃ Link Data                                   ┃
    ┠-----------+---------------+-----------------┨
    ┃ Link Type | Number of Tos | Metric          ┃
    ┠-----------+---------------+-----------------┨
    ┃ Tos       | 0x00          | Tos Metric      ┃
    ┠-----------+---------------+-----------------┨
    ┃                    ...                      ┃
    ┠---------------------------------------------┨
    ┃ Link ID                                     ┃
    ┠---------------------------------------------┨
    ┃ Link Data                                   ┃
    ┗---------------------------------------------┛

| Link Type | Link connected to:              | Link ID Value                           | Link Data Value                    |
|-----------|---------------------------------|-----------------------------------------|------------------------------------|
|         1 | another Router (point-to-point) | neighboring router's ID                 | 和网络相连的始发路由器接口的ipaddr |
|         2 | a Transit Network               | designated router's interface's IP addr | 和网络相连的始发路由器接口的ipaddr |
|         3 | a Stub Network                  | IP网络/子网地址                         | 网络ip addr / subnet mask          |
|         4 | virtual link                    | neighboring router's ID                 | 始发路由器接口的mib-ii ifindex值   |

#### OSPFv2 Network LSA

    ┃ 16bit    |  8bit  | 8bit ┃
    ┏----------+--------+------┓
    ┃ Age      | Option |Type=2┃
    ┠----------+--------+------┨
    ┃ Link State ID            ┃
    ┠--------------------------┨
    ┃ Advertising Router       ┃
    ┠--------------------------┨
    ┃ Sequence                 ┃
    ┠----------+---------------┨
    ┃ Checksum | Length        ┃
    ┠----------+---------------┨
    ┃ Network Mask             ┃
    ┠--------------------------┨
    ┃ Attached Router          ┃
    ┠--------------------------┨
    ┃           ...            ┃
    ┠--------------------------┨
    ┃ Attached Router          ┃
    ┗--------------------------┛

#### OSPFv2 Network/ASBR Summary LSA

    ┃ 8bit | 8bit | 8bit   | 8bit     ┃
    ┏-------------+--------+----------┓
    ┃ Age         | Option | Type=3/4 ┃
    ┠-------------+--------+----------┨
    ┃ Link State ID                   ┃
    ┠---------------------------------┨
    ┃ Advertising Router              ┃
    ┠---------------------------------┨
    ┃ Sequence                        ┃
    ┠-------------+-------------------┨
    ┃ Checksum    | Length            ┃
    ┠-------------+-------------------┨
    ┃ Network Mask                    ┃
    ┠------+--------------------------┨
    ┃ 0x00 | Metric                   ┃
    ┠------+--------------------------┨
    ┃ Tos  | Tos Metric               ┃
    ┠------+--------------------------┨
    ┃               ...               ┃
    ┠------+--------------------------┨
    ┃ 0x00 | Metric                   ┃
    ┗---------------------------------┛

#### OSPFv2 Autonomous System External LSA

    ┃ 8bit    | 8bit | 8bit   | 8bit   ┃
    ┏----------------+--------+--------┓
    ┃ Age            | Option | Type=5 ┃
    ┠----------------+--------+--------┨
    ┃ Link State ID                    ┃
    ┠----------------------------------┨
    ┃ Advertising Router               ┃
    ┠----------------------------------┨
    ┃ Sequence                         ┃
    ┠----------------+-----------------┨
    ┃ Checksum       | Length          ┃
    ┠----------------+-----------------┨
    ┃ Network Mask                     ┃
    ┠-+-------+------------------------┨
    ┃E|0000000| Metric                 ┃
    ┠-+-------+------------------------┨
    ┃ Forwarding Address               ┃
    ┠----------------------------------┨
    ┃ External Route Tag               ┃
    ┠-+-------+------------------------┨
    ┃E|  Tos  | Metric                 ┃
    ┠-+-------+------------------------┨
    ┃ Forwarding Address               ┃
    ┠----------------------------------┨
    ┃ External Route Tag               ┃
    ┠----------------------------------┨
    ┃                ...               ┃
    ┠----------------------------------┨
    ┃ Network Mask                     ┃
    ┠-+-------+------------------------┨
    ┃E|0000000| Metric                 ┃
    ┠-+-------+------------------------┨
    ┃ Forwarding Address               ┃
    ┠----------------------------------┨
    ┃ External Route Tag               ┃
    ┗----------------------------------┛
	E:External Metric bit:0(E1)/1(E2)

#### OSPFv2 Not-So-Stubby Area External LSA

    ┃ 8bit    | 8bit | 8bit   | 8bit   ┃
    ┏----------------+--------+--------┓
    ┃ Age            | Option | Type=5 ┃
    ┠----------------+--------+--------┨
    ┃ Link State ID                    ┃
    ┠----------------------------------┨
    ┃ Advertising Router               ┃
    ┠----------------------------------┨
    ┃ Sequence                         ┃
    ┠----------------+-----------------┨
    ┃ Checksum       | Length          ┃
    ┠----------------+-----------------┨
    ┃ Network Mask                     ┃
    ┠-+-------+------------------------┨
    ┃E|  Tos  | Metric                 ┃
    ┠-+-------+------------------------┨
    ┃ Forwarding Address               ┃
    ┠----------------------------------┨
    ┃ External Route Tag               ┃
    ┠----------------------------------┨
    ┃                ...               ┃
    ┠----------------------------------┨
    ┃ Network Mask                     ┃
    ┠-+-------+------------------------┨
    ┃E|  Tos  | Metric                 ┃
    ┠-+-------+------------------------┨
    ┃ Forwarding Address               ┃
    ┠----------------------------------┨
    ┃ External Route Tag               ┃
    ┗----------------------------------┛
	E:External Metric bit:0(E1)/1(E2)

