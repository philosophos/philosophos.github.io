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
- OSPF Router LSA
- OSPF Network LSA
- OSPF Network/ASBR Summary LSA
- OSPF Autonomous System External LSA
- OSPF Not-So-Stubby Area External LSA

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

| number | type                           | Command                             |
|--------|--------------------------------|-------------------------------------|
|      1 | Router LSA                     | show ip ospf database router        |
|      2 | Network LSA                    | show ip ospf database network       |
|      3 | Network Summary LSA            | show ip ospf database summary       |
|      4 | ASBR Summary LSA               | show ip ospf database asbr-summary  |
|      5 | AS External LSA                | show ip ospf database external      |
|      6 | Group Membership LSA           |                                     |
|      7 | NSSA External LSA              | show ip ospf database nssa-external |
|      8 | External Attributes LSA        | show ip ospf database               |
|      9 | Opaque LSA (link链路本地范围 ) |                                     |
|     10 | Opaque LSA (area本地区域范围)  |                                     |
|     11 | Opaque LSA (AS范围)            |                                     |

#### OSPF Router LSA

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

#### OSPF Network LSA

    ┃ 16bit    |  8bit  | 8bit ┃
    ┏----------+--------+------┓
    ┃ age      | option |type=2┃
    ┠----------+--------+------┨
    ┃ link state id            ┃
    ┠--------------------------┨
    ┃ advertising router       ┃
    ┠--------------------------┨
    ┃ sequence                 ┃
    ┠----------+---------------┨
    ┃ checksum | length        ┃
    ┠----------+---------------┨
    ┃ network mask             ┃
    ┠--------------------------┨
    ┃ attached router          ┃
    ┠--------------------------┨
    ┃ attached router          ┃
    ┠--------------------------┨
    ┃           ...            ┃
    ┠--------------------------┨
    ┃ attached router          ┃
    ┗--------------------------┛

#### OSPF Network/ASBR Summary LSA

    ┃ 8bit | 8bit | 8bit   | 8bit     ┃
    ┏-------------+--------+----------┓
    ┃ age         | option | type=3/4 ┃
    ┠-------------+--------+----------┨
    ┃ link state id                   ┃
    ┠---------------------------------┨
    ┃ advertising router              ┃
    ┠---------------------------------┨
    ┃ sequence                        ┃
    ┠-------------+-------------------┨
    ┃ checksum    | length            ┃
    ┠-------------+-------------------┨
    ┃ network mask                    ┃
    ┠------+--------------------------┨
    ┃ 0x00 | metric                   ┃
    ┠------+--------------------------┨
    ┃ tos  | tos metric               ┃
    ┠------+--------------------------┨
    ┃               ...               ┃
    ┠------+--------------------------┨
    ┃ 0x00 | metric                   ┃
    ┗---------------------------------┛

#### OSPF Autonomous System External LSA

    ┃ 8bit    | 8bit | 8bit   | 8bit   ┃
    ┏----------------+--------+--------┓
    ┃ age            | option | type=5 ┃
    ┠----------------+--------+--------┨
    ┃ link state id                    ┃
    ┠----------------------------------┨
    ┃ advertising router               ┃
    ┠----------------------------------┨
    ┃ sequence                         ┃
    ┠----------------+-----------------┨
    ┃ checksum       | length          ┃
    ┠----------------+-----------------┨
    ┃ network mask                     ┃
    ┠-+-------+------------------------┨
    ┃e|0000000| metric                 ┃
    ┠-+-------+------------------------┨
    ┃ forwarding address               ┃
    ┠----------------------------------┨
    ┃ external route tag               ┃
    ┠-+-------+------------------------┨
    ┃e|  tos  | tos metric             ┃
    ┠-+-------+------------------------┨
    ┃ forwarding address               ┃
    ┠----------------------------------┨
    ┃ external route tag               ┃
    ┠----------------------------------┨
    ┃                ...               ┃
    ┠----------------+-----------------┨
    ┃ network mask                     ┃
    ┠-+-------+------------------------┨
    ┃e|0000000| metric                 ┃
    ┠-+-------+------------------------┨
    ┃ forwarding address               ┃
    ┠----------------------------------┨
    ┃ external route tag               ┃
    ┗----------------------------------┛
	e:external metric bit:0(e1)/1(e2)

#### OSPF Not-So-Stubby Area External LSA

    ┃ 8bit    | 8bit | 8bit   | 8bit   ┃
    ┏----------------+--------+--------┓
    ┃ age            | option | type=5 ┃
    ┠----------------+--------+--------┨
    ┃ link state id                    ┃
    ┠----------------------------------┨
    ┃ advertising router               ┃
    ┠----------------------------------┨
    ┃ sequence                         ┃
    ┠----------------+-----------------┨
    ┃ checksum       | length          ┃
    ┠----------------+-----------------┨
    ┃ network mask                     ┃
    ┠-+-------+------------------------┨
    ┃e|  tos  | tos metric             ┃
    ┠-+-------+------------------------┨
    ┃ forwarding address               ┃
    ┠----------------------------------┨
    ┃ external route tag               ┃
    ┠----------------------------------┨
    ┃                ...               ┃
    ┠----------------+-----------------┨
    ┃ network mask                     ┃
    ┠-+-------+------------------------┨
    ┃e|0000000| metric                 ┃
    ┠-+-------+------------------------┨
    ┃ forwarding address               ┃
    ┠----------------------------------┨
    ┃ external route tag               ┃
    ┗----------------------------------┛
	e:external metric bit:0(e1)/1(e2)

