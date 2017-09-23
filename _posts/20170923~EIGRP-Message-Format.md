---
layout: post
title: EIGRP Message Format
copyright: true
date: 2017-09-23 20:23:39
updated:
description:
categories:
- Network
tags:
- Network
- PDU
- Routing
- EIGRP
---

EIGRP Message Format
TLV(type/length/value)
- TLV with EIGRP参数
- IP Internal Routing's TLV
- IP External Routing's TLV
External Protocol ID

<!-- more -->

-----------------------------------------------------------
EIGRP Message Format : IP 88
-----------------------------------------------------------

    ┃ 8bit    | 8bit   | 16bit    ┃
    ┏---------+--------+----------┓ ----|
    ┃ Version | Opcode | Checksum ┃     |
    ┠---------+--------+----------┨     |
    ┃ Flags                       ┃  20byte
    ┠-----------------------------┨     |
    ┃ Sequence                    ┃     |
    ┠-----------------------------┨     |
    ┃ ACK                         ┃     |
    ┠-----------------------------┨     |
    ┃ Autonomous System Number    ┃     |
    ┠-----------------------------┨ ----|
    ┃ TLV(type/length/value)      ┃
    ┗-----------------------------┛
| number | TLV(type/length/value)     |
|--------|----------------------------|
| 0x0001 | EIGRP参数                  |
| 0x0003 | Sequence                   |
| 0x0004 | Software Version           |
| 0x0005 | Next Multicast Sequence    |
| 0x0102 | IP Internal Routing        |
| 0x0103 | IP External Routing        |
| 0x0202 | AppleTalk Internal Routing |
| 0x0203 | AppleTalk External Routing |
| 0x0204 | AppleTalk link config      |
| 0x0302 | IPX Internal Routing       |
| 0x0303 | IPX External Routing       |

#### TLV with EIGRP参数

    ┃ 8bit |  8bit  | 8bit | 8bit ┃
    ┏---------------+-------------┓
	┃ type=0x0001   | length      ┃
    ┠------+--------+------+------┨
    ┃ K1   | K2     | K3   | K4   ┃
    ┠------+--------+------+------┨
	┃ K5   |Reserved| hold time   ┃
    ┗------+--------+-------------┛

#### IP Internal Routing's TLV

    ┃ 8bit        | 8bit | 8bit | 8bit    ┃
    ┏--------------------+----------------┓
	┃ type=0x0102        | length         ┃
    ┠--------------------+----------------┨
    ┃ Next Hop                            ┃
    ┠-------------------------------------┨
    ┃ Delay                               ┃
    ┠-------------------------------------┨
    ┃ Bandwidth                           ┃
    ┠---------------------------+---------┨
    ┃ MTU                       |Hop Count┃
    ┠-------------+------+------+---------┨
    ┃ Reliability | Load | Reserved       ┃
    ┠-------------+------+----------------┨
    ┃Prefix Length| Destination           ┃
    ┗-------------+-----------------------┛

#### IP External Routing's TLV

    ┃ 8bit        | 8bit |        8bit        |   8bit  ┃
    ┏--------------------+------------------------------┓
	┃ type=0x0103        | length                       ┃
    ┠--------------------+------------------------------┨
    ┃ Next Hop                                          ┃
    ┠---------------------------------------------------┨
    ┃ Originating Router                                ┃
    ┠---------------------------------------------------┨
    ┃ Originating Autonomous System Number              ┃
    ┠---------------------------------------------------┨
    ┃ Arbitrary Tag                                     ┃
    ┠---------------------------------------------------┨
    ┃ External Protocol Metric                          ┃
    ┠--------------------+--------------------+---------┨
    ┃ Reserved           |External Protocol ID| Flags   ┃
    ┠--------------------+--------------------+---------┨
    ┃ Delay                                             ┃
    ┠---------------------------------------------------┨
    ┃ Bandwidth                                         ┃
    ┠-----------------------------------------+---------┨
    ┃ MTU                                     |Hop Count┃
    ┠-------------+------+--------------------+---------┨
    ┃ Reliability | Load | Reserved                     ┃
    ┠-------------+------+------------------------------┨
    ┃Prefix Length| Destination                         ┃
    ┗-------------+-------------------------------------┛
External Protocol ID:

| ID | External Protocol 	  |
|--------|------------------------|
|   0x01 | IGRP                   |
|   0x02 | EIGRP                  |
|   0x03 | Static Route           |
|   0x04 | RIP                    |
|   0x05 | Hello                  |
|   0x06 | OSPF                   |
|   0x07 | IS-IS                  |
|   0x08 | EGP                    |
|   0x09 | BGP                    |
|   0x0a | IDRP                   |
|   0x0b | Direct Connect         |

