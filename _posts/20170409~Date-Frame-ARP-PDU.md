---
layout: post
title: Date Frame & ARP PDU
copyright: true
date: 2017-04-09 14:56:57
updated:
description:
categories:
- Network
tags:
- Network
- Frame
- ARP
---

- Data Frame
    - Ethernet Frame(RFC 894)
    - IEEE 802.3 Frame(RFC 1042)
- ARP(Address Resolution Protocol) PDU
<!-- more -->

-----------------------------
Data Frame
-----------------------------
### Ethernet Frame(RFC 894)
    ┃ 8byte        │ 6         │ 6        │ 2      │0~1500│ 46~0    │ 4   ┃
    ┏--------------┯-----------┯----------┯--------┯------┯---------┯-----┓
    ┃Preamble      │ dest addr │ src addr │ type   │ data │ padding │ FCS ┃
    ┗--------------┷-----------┷----------┷--------┷------┷---------┷-----┛
Preamble: 10101010 10101010 10101010 10101010 10101010 10101010 10101010 10101011 

### IEEE 802.3 Frame(RFC 1042)
    ┃ 7byte  │ 1   │ 6         │ 6        │ 2      │0~1500│ 46~0    │ 4   ┃
    ┏--------┯-----┯-----------┯----------┯--------┯------┯---------┯-----┓
    ┃Preamble│ SOF │ dest addr │ src addr │ length │ data │ padding │ FCS ┃
    ┗--------┷-----┷-----------┷----------┷--------┷------┷---------┷-----┛
                                                    /               \

            │ 1byte│ 1byte│ 1byte│ 3byte    │ 2byte│0~1492│38~0     │
            ┍------┯------┯------┯----------┯------┯------┯---------┑
            │ DSAP │ SSAP │ ctrl │ org code │ type │ date │ padding │
            │ 0xaa │ 0xaa │ 0x03 │ 0x000000 │      │      │         │
            ┕------┷------┷------┷----------┷------┷------┷---------┙
Preamble: 10101010 10101010 10101010 10101010 10101010 10101010 10101010 
SOF,Start-Of-Frame: 10101011

Type:0x0800--IP, 0x0806--ARP Request/Reply, 0x0835--RARP Request/Reply
FCS,Frame Check Sequence,CRC

-----------------------------
ARP(Address Resolution Protocol) PDU
-----------------------------
    ┃- 2byte-│- 2byte │-- 1byte --│-- 1byte --│- 2byte -│ 6byte │ 4byte│ 6byte  │ 4byte ┃
    ┏--------┯--------┯-----------┯-----------┯---------┯-------┯------┯--------┯-------┓
    ┃Hardware│Protocol│ Hardware  │ Protocol  │Operation│src mac│src IP│dest mac│dest IP┃ 
    ┃ Type   │ Type   │Addr Length│Addr Length│         │ addr  │ addr │ addr   │ addr  ┃
    ┗--------┷--------┷-----------┷-----------┷---------┷-------┷------┷--------┷-------┛
Hardware Type:
1  -- ethernet
3  -- X.25
4  -- Proteon ProNet Token Ring
6  -- IEEE 802 network
7  -- ARCnet
11 -- Apple LocalTalk
14 -- SMDS
15 -- Frame Relay
16 -- ATM
17 -- HDLC
18 -- Fiber channel
19 -- ATM
20 -- Serial Link

Protocol Type:
IP -- 0x0800

Operation:
1 -- ARP Request
2 -- ARP Reply
3 -- RARP Request
4 -- RARP Reply
5 -- Dynamic RARP Request
6 -- Dynamic RARP Reply
7 -- Dynamic RARP Error
8 -- InARP Request
9 -- InARP Reply

