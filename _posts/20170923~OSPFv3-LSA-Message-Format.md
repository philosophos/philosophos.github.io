---
layout: post
title: OSPFv3 LSA Message Format
copyright: true
date: 2017-09-23 20:24:57
updated:
description:
categories:
- Network
tags:
- Network
- PDU
- Routing
- OSPF
- OSPFv3
- LSA
---

OSPFv3 Protocol LSA Header
- OSPFv3 Router LSA
- OSPFv3 Network LSA
- OSPFv3 Inter-Area Prefix LSA
- OSPFv3 Inter-Area Router LSA
- OSPFv3 AS External LSA
- OSPFv3 Link LSA
- OSPFv3 Intra-Area Prefix LSA
Prefix Option

<!-- more -->

-----------------------------------------------------------
OSPFv3 Protocol LSA Header
-----------------------------------------------------------

    ┃ 16bit    | 16bit   ┃
    ┏----------+---------┓
    ┃ Age      | Type    ┃
    ┠----------+---------┨
    ┃ Link State ID      ┃
    ┠--------------------┨
    ┃ Advertising Router ┃
    ┠--------------------┨
    ┃ Sequence           ┃
    ┠----------+---------┨
    ┃ Checksum | Length  ┃
    ┗----------+---------┛

Link State Type

    ┏---+----+----+-------------------┓
    ┃ U | S2 | S1 | LSA Function Code ┃
    ┗---+----+----+-------------------┛

| S2 | S1 | Flooding Diffusion Ranger |
|----|----|---------------------------|
|  0 |  0 | 链路本地                  |
|  0 |  1 | 区域                      |
|  1 |  0 | AS                        |
|  1 |  1 | Reserved                  |


|   Type | OSPFv3 Type Name      | Type | OSPFv2 Type Name     | Command                             |
|--------|-----------------------|------|----------------------|-------------------------------------|
| 0x2001 | Router LSA            |    1 | Router LSA           | show ip ospf database router        |
| 0x2002 | Network LSA           |    2 | Network LSA          | show ip ospf database network       |
| 0x2003 | Inter-Area Prefix LSA |    3 | Network Summary LSA  | show ip ospf database summary       |
| 0x2004 | Inter-Area Router LSA |    4 | ASBR Summary LSA     | show ip ospf database asbr-summary  |
| 0x4005 | AS External LSA       |    5 | AS External LSA      | show ip ospf database external      |
| 0x2006 | Group Membership      |    6 | Group Membership LSA |                                     |
| 0x2007 | Type7 LSA             |    7 | NSSA External LSA    | show ip ospf database nssa-external |
| 0x0008 | Link LSA              |      | no corresponding LSA |                                     |
| 0x2009 | Intra-Area Prefix LSA |      | no corresponding LSA |                                     |

#### OSPFv3 Router LSA

    ┃ 8bit           | 8bit |    16bit    ┃
    ┏-----------------------+-------------┓
    ┃ Age                   | Type=0x2001 ┃
    ┠-----------------------+-------------┨
    ┃ Link State ID                       ┃
    ┠-------------------------------------┨
    ┃ Advertising Router                  ┃
    ┠-------------------------------------┨
    ┃ Link State Sequence                 ┃
    ┠-----------------------+-------------┨
    ┃ Link State Checksum   | Length      ┃
    ┠------+---------+------+-------------┨
    ┃ 0000 | W|V|E|B | Option             ┃ 
    ┠------+---------+------+-------------┨
    ┃ Interface Type | 0x00 | Metric      ┃
    ┠----------------+------+-------------┨
    ┃ 实例 ID                             ┃
    ┠-------------------------------------┨
    ┃ Neighbor Interface ID               ┃
    ┠-------------------------------------┨
    ┃ Neighbor Router ID                  ┃
    ┠-------------------------------------┨
    ┃                 ...                 ┃
    ┠----------------+------+-------------┨
    ┃ Interface Type | 0x00 | Metric      ┃
    ┠----------------+------+-------------┨
    ┃ 实例 ID                             ┃
    ┠-------------------------------------┨
    ┃ Neighbor Interface ID               ┃
    ┠-------------------------------------┨
    ┃ Neighbor Router ID                  ┃
    ┗-------------------------------------┛

| Interface Type | Link connected to:              |
|----------------|---------------------------------|
|              1 | another Router (point-to-point) |
|              2 | a Transit Network               |
|              3 | Reserved                        |
|              4 | virtual link                    |

#### OSPFv3 Network LSA

    ┃ 8bit | 8bit |    16bit    ┃
    ┏-------------+-------------┓
    ┃ Age         | Type=0x2002 ┃
    ┠-------------+-------------┨
    ┃ Link State ID             ┃
    ┠---------------------------┨
    ┃ Advertising Router        ┃
    ┠---------------------------┨
    ┃ Link State Sequence       ┃
    ┠-------------+-------------┨
    ┃ Checksum    | Length      ┃
    ┠------+------+-------------┨
    ┃ 0x00 | Option             ┃ 
    ┠------+--------------------┨
    ┃ Attached Router           ┃
    ┠---------------------------┨
    ┃           ...             ┃
    ┠---------------------------┨
    ┃ Attached Router           ┃
    ┗---------------------------┛

#### OSPFv3 Inter-Area Prefix LSA

    ┃ 8bit          | 8bit          |    16bit    ┃
    ┏-------------------------------+-------------┓
    ┃ Age                           | Type=0x2003 ┃
    ┠-------------------------------+-------------┨
    ┃ Link State ID                               ┃
    ┠---------------------------------------------┨
    ┃ Advertising Router                          ┃
    ┠---------------------------------------------┨
    ┃ Link State Sequence                         ┃
    ┠-------------------------------+-------------┨
    ┃ Checksum                      | Length      ┃
    ┠---------------+---------------+-------------┨
    ┃ 0x00          | Metric                      ┃
    ┠---------------+---------------+-------------┨
    ┃ Prefix Length | Prefix Option | 0x0000      ┃
    ┠---------------+---------------+-------------┨
    ┃ IPaddr Prefix                               ┃
    ┃ ...                                         ┃
    ┗---------------------------------------------┛

#### OSPFv3 Inter-Area Router LSA

    ┃ 8bit | 8bit |    16bit    ┃
    ┏-------------+-------------┓
    ┃ Age         | Type=0x2004 ┃
    ┠-------------+-------------┨
    ┃ Link State ID             ┃
    ┠---------------------------┨
    ┃ Advertising Router        ┃
    ┠---------------------------┨
    ┃ Link State Sequence       ┃
    ┠-------------+-------------┨
    ┃ Checksum    | Length      ┃
    ┠------+------+-------------┨
    ┃ 0x00 | Option             ┃
    ┠------+--------------------┨
    ┃ 0x00 | Metric             ┃
    ┠------+--------------------┨
    ┃ Destination Router ID     ┃
    ┗---------------------------┛
	Destination Router ID:ASBR's RID

#### OSPFv3 AS External LSA

    ┃ 8bit          | 8bit          |    16bit       ┃
    ┏-------------------------------+----------------┓
    ┃ Age                           | Type=0x4005    ┃
    ┠-------------------------------+----------------┨
    ┃ Link State ID                                  ┃
    ┠------------------------------------------------┨
    ┃ Advertising Router                             ┃
    ┠------------------------------------------------┨
    ┃ Link State Sequence                            ┃
    ┠-------------------------------+----------------┨
    ┃ Checksum                      | Length         ┃
    ┠---------------+---------------+----------------┨
    ┃ 00000 | E|F|T | Metric                         ┃
    ┠---------------+---------------+----------------┨
    ┃ Prefix Length | Prefix Option | Interface Type ┃
    ┠---------------+---------------+----------------┨
    ┃ IPaddr Prefix                                  ┃
    ┃ ...                                            ┃
    ┠------------------------------------------------┨
    ┃ Transit IPaddr (if F=1)                        ┃
    ┠------------------------------------------------┨
    ┃ External Route Tag (if T=1)                    ┃
    ┠------------------------------------------------┨
    ┃ Reference Link State ID (option)               ┃
    ┗------------------------------------------------┛
	E:External Metric bit:0(E1)/1(E2)

#### OSPFv3 Link LSA

    ┃ 8bit          | 8bit            |    16bit    ┃
    ┏---------------------------------+-------------┓
    ┃ Age                             | Type=0x0008 ┃
    ┠---------------------------------+-------------┨
    ┃ Link State ID                                 ┃
    ┠-----------------------------------------------┨
    ┃ Advertising Router                            ┃
    ┠-----------------------------------------------┨
    ┃ Link State Sequence                           ┃
    ┠---------------------------------+-------------┨
    ┃ Checksum                        | Length      ┃
    ┠-----------------+---------------+-------------┨
    ┃ Router Priority | Option                      ┃
    ┠-----------------+-----------------------------┨
    ┃ 链路本地 Prefix地址                          ┃
    ┃ ...                                           ┃
    ┠-----------------------------------------------┨
    ┃ Number of Prefix                              ┃
    ┠-----------------+---------------+-------------┨
    ┃ Prefix Length   | Prefix Option | 0x0000      ┃
    ┠-----------------+---------------+-------------┨
    ┃ IPaddr Prefix                                 ┃
    ┃ ...                                           ┃
    ┠-----------------------------------------------┨
    ┃ ...                                           ┃
    ┠-----------------------------------------------┨
    ┃ Number of Prefix                              ┃
    ┠-----------------+---------------+-------------┨
    ┃ Prefix Length   | Prefix Option | 0x0000      ┃
    ┠-----------------+---------------+-------------┨
    ┃ IPaddr Prefix                                 ┃
    ┃ ...                                           ┃
    ┗-----------------------------------------------┛

#### OSPFv3 Intra-Area Prefix LSA

    ┃ 8bit          | 8bit          | 16bit                     ┃
    ┏-------------------------------+---------------------------┓
    ┃ Age                           | Type=0x2009               ┃
    ┠-------------------------------+---------------------------┨
    ┃ Link State ID                                             ┃
    ┠-----------------------------------------------------------┨
    ┃ Advertising Router                                        ┃
    ┠-----------------------------------------------------------┨
    ┃ Link State Sequence                                       ┃
    ┠-------------------------------+---------------------------┨
    ┃ Checksum                      | Reference Link State Type ┃
    ┠-------------------------------+---------------------------┨
    ┃ Reference Link State ID                                   ┃
    ┠-----------------------------------------------------------┨
    ┃ Reference Advertising Router                              ┃
    ┠---------------+---------------+---------------------------┨
    ┃ Prefix Length | Prefix Option | Metric                    ┃
    ┠---------------+---------------+---------------------------┨
    ┃ IPaddr Prefix                                             ┃
    ┃ ...                                                       ┃
    ┠-----------------------------------------------------------┨
    ┃ ...                                                       ┃
    ┠---------------+---------------+---------------------------┨
    ┃ Prefix Length | Prefix Option | Metric                    ┃
    ┠---------------+---------------+---------------------------┨
    ┃ IPaddr Prefix                                             ┃
    ┃ ...                                                       ┃
    ┗-----------------------------------------------------------┛

Prefix Option

    ┏-+-+-+-+---+----+----+----┓
    ┃ | | | | P | MC | LA | NU ┃
    ┗-+-+-+-+---+----+----+----┛
P :Propagate,传播位
MC:MultiCast,多播位
LA:Local Address,本地地址位
NU:No Unicast,无单播位
