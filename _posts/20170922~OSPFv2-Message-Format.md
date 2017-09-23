---
layout: post
title: OSPFv2 Message Format
copyright: true
date: 2017-09-22 20:24:21
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
---

OSPFv2 Message Format
OSPF Message Type
OSPF Authentication Type
- OSPF Hello packet
- OSPF database description packet
- OSPF link state request packet
- OSPF link state update packet
- OSPF link state confirm packet

<!-- more -->

-----------------------------------------------------------
OSPFv2 Message Format
-----------------------------------------------------------

    ┃ 8bit    | 8bit |    16bit    ┃
    ┏---------+------+-------------┓
	┃Version=2| Type |Packet Length┃
    ┠---------+------+-------------┨
    ┃ Router ID                    ┃
    ┠------------------------------┨
    ┃ Area ID                      ┃
    ┠----------------+-------------┨
    ┃ Checksum       | AuType      ┃
    ┠----------------+-------------┨
    ┃ Authentication               ┃
    ┠------------------------------┨
    ┃ Authentication               ┃
    ┠------------------------------┨
    ┃ Data                         ┃
    ┗------------------------------┛

if AuType=2

    ┠--------+--------+----------------------------┨
    ┃ 0x0000 | Key ID | Authentication Data Length ┃
    ┠--------+--------+----------------------------┨
    ┃ Cryptographic Sequence Number                ┃
    ┠----------------------------------------------┨

OSPF Message Type

| number | OSPF message type    |
|--------|----------------------|
|      1 | Hello                |
|      2 | database description |
|      3 | link state request   |
|      4 | link state update    |
|      5 | link state confirm   |

OSPF Authentication Type

| number | OSPF  Authentication type                  |
|--------|--------------------------------------------|
|      0 | null (no authentication)                   |
|      1 | simple password authentication (plaintext) |
|      2 | cryptographic checksum (MD5)               |

#### OSPF Hello packet

    ┃ 8bit    | 8bit |  8bit  | 8bit           ┃
    ┏---------+------+-------------------------┓
	┃Version=2|Type=1| Packet Length           ┃
    ┠---------+------+-------------------------┨
    ┃ Router ID                                ┃
    ┠------------------------------------------┨
    ┃ Area ID                                  ┃
    ┠----------------+-------------------------┨
    ┃ Checksum       | AuType                  ┃
    ┠----------------+-------------------------┨
    ┃ Authentication                           ┃
    ┠------------------------------------------┨
    ┃ Authentication                           ┃
    ┠------------------------------------------┨
    ┃ Network Mask                             ┃
    ┠----------------+--------+----------------┨
    ┃ Hello Interval | Option | Router Priority┃
    ┠----------------+--------+----------------┨
    ┃ Router Dead Interval                     ┃
    ┠------------------------------------------┨
    ┃ Designated Router                        ┃
    ┠------------------------------------------┨
    ┃ Back-up Designated Router                ┃
    ┠------------------------------------------┨
    ┃ Neighbor                                 ┃
    ┠------------------------------------------┨
    ┃                   ...                    ┃
    ┠------------------------------------------┨
    ┃ Neighbor                                 ┃
    ┗------------------------------------------┛

#### OSPF database description packet

    ┃ 8bit    | 8bit |  8bit  | 8bit       ┃
    ┏---------+------+---------------------┓
	┃Version=2|Type=2| Packet Length       ┃
    ┠---------+------+---------------------┨
    ┃ Router ID                            ┃
    ┠--------------------------------------┨
    ┃ Area ID                              ┃
    ┠----------------+---------------------┨
    ┃ Checksum       | AuType              ┃
    ┠----------------+---------------------┨
    ┃ Authentication                       ┃
    ┠--------------------------------------┨
    ┃ Authentication                       ┃
    ┠----------------+--------+-----+-+-+--┨
    ┃ Interface MTU  | Option |00000|I|M|MS┃
    ┠----------------+--------+-----+-+-+--┨
    ┃ DD Sequence Number                   ┃
    ┠--------------------------------------┨
    ┃ LSA Header                           ┃
    ┗--------------------------------------┛
	I:Initial bit
	M:More bit
	MS:Master/Slave bit

#### OSPF link state request packet

    ┃ 8bit    | 8bit |     16bit     ┃
    ┏---------+------+---------------┓
	┃Version=2|Type=3| Packet Length ┃
    ┠---------+------+---------------┨
    ┃ Router ID                      ┃
    ┠--------------------------------┨
    ┃ Area ID                        ┃
    ┠----------------+---------------┨
    ┃ Checksum       | AuType        ┃
    ┠----------------+---------------┨
    ┃ Authentication                 ┃
    ┠--------------------------------┨
    ┃ Authentication                 ┃
    ┠--------------------------------┨
    ┃ Link State Type                ┃
    ┠--------------------------------┨
    ┃ Link State ID                  ┃
    ┠--------------------------------┨
    ┃ Advertising Router             ┃
    ┠--------------------------------┨
    ┃              ...               ┃
    ┠--------------------------------┨
    ┃ Link State Type                ┃
    ┠--------------------------------┨
    ┃ Link State ID                  ┃
    ┠--------------------------------┨
    ┃ Advertising Router             ┃
    ┗--------------------------------┛

#### OSPF link state update packet

    ┃ 8bit    | 8bit |     16bit     ┃
    ┏---------+------+---------------┓
	┃Version=2|Type=4| Packet Length ┃
    ┠---------+------+---------------┨
    ┃ Router ID                      ┃
    ┠--------------------------------┨
    ┃ Area ID                        ┃
    ┠----------------+---------------┨
    ┃ Checksum       | AuType        ┃
    ┠----------------+---------------┨
    ┃ Authentication                 ┃
    ┠--------------------------------┨
    ┃ Authentication                 ┃
    ┠--------------------------------┨
    ┃ Number of LSA                  ┃
    ┠--------------------------------┨
    ┃ LSA                            ┃
    ┗--------------------------------┛

#### OSPF link state confirm packet

    ┃ 8bit    | 8bit |     16bit     ┃
    ┏---------+------+---------------┓
	┃Version=2|Type=4| Packet Length ┃
    ┠---------+------+---------------┨
    ┃ Router ID                      ┃
    ┠--------------------------------┨
    ┃ Area ID                        ┃
    ┠----------------+---------------┨
    ┃ Checksum       | AuType        ┃
    ┠----------------+---------------┨
    ┃ Authentication                 ┃
    ┠--------------------------------┨
    ┃ Authentication                 ┃
    ┠--------------------------------┨
    ┃ LSA Header                     ┃
    ┗--------------------------------┛

