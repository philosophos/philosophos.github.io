---
layout: post
title: OSPFv3 Message Format
copyright: true
date: 2017-09-22 20:24:27
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
---

OSPFv3 Message Format
- OSPFv3 Hello packet
- OSPFv3 database description packet

<!-- more -->

-----------------------------------------------------------
OSPFv3 Message Format
-----------------------------------------------------------

    ┃ 8bit    | 8bit |    8bit | 8bit ┃
    ┏---------+------+----------------┓
	┃Version=3| Type | Packet Length  ┃
    ┠---------+------+----------------┨
    ┃ Router ID                       ┃
    ┠---------------------------------┨
    ┃ Area ID                         ┃
    ┠----------------+---------+------┨
    ┃ Checksum       | 实例ID | 0x00 ┃
    ┠----------------+---------+------┨

#### OSPFv3 Hello packet

    ┃ 8bit    | 8bit |  8bit  | 8bit        ┃
    ┏---------+------+----------------------┓
	┃Version=3|Type=1| Packet Length        ┃
    ┠---------+------+----------------------┨
    ┃ Router ID                             ┃
    ┠---------------------------------------┨
    ┃ Area ID                               ┃
    ┠----------------+---------+------------┨
    ┃ Checksum       | 实例ID | 0x00       ┃
    ┠----------------+---------+------------┨
    ┃ Hello Interval | Router Dead Interval ┃
    ┠----------------+----------------------┨
    ┃ Designated Router                     ┃
    ┠---------------------------------------┨
    ┃ Back-up Designated Router             ┃
    ┠---------------------------------------┨
    ┃ Neighbor                              ┃
    ┠---------------------------------------┨
    ┃                   ...                 ┃
    ┠---------------------------------------┨
    ┃ Neighbor                              ┃
    ┗---------------------------------------┛

#### OSPFv3 database description packet

    ┃ 8bit    | 8bit |  8bit  | 8bit       ┃
    ┏---------+------+---------------------┓
	┃Version=3|Type=2| Packet Length       ┃
    ┠---------+------+---------------------┨
    ┃ Router ID                            ┃
    ┠--------------------------------------┨
    ┃ Area ID                              ┃
    ┠----------------+---------+-----------┨
    ┃ Checksum       | 实例ID | 0x00      ┃
    ┠---------+------+---------+-----------┨
    ┃ 0x00    | Option                     ┃
    ┠---------+------+--------+-----+-+-+--┨
    ┃ Interface MTU  | 0x00   |00000|I|M|MS┃
    ┠----------------+--------+-----+-+-+--┨
    ┃ DD Sequence Number                   ┃
    ┠--------------------------------------┨
    ┃ LSA Header                           ┃
    ┗--------------------------------------┛
	I:Initial bit
	M:More bit
	MS:Master/Slave bit
