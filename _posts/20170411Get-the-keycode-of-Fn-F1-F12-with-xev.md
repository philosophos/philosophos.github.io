---
layout: post
title: Get the keycode of Fn+F1~F12 with xev
copyright: true
date: 2017-04-11 20:37:59
updated:
description:
categories:
- Linux
tags:
- Linux
- keycode
- xev
- Fn
---

`xev` can print contents of X events, but can't getting the event for the keypress of `Fn+F1~F12`.
<!-- more -->
The following is the output when press and release `Fn+F1~F12`.

    FocusOut event, serial 35, synthetic NO, window 0x1800001,
        mode NotifyGrab, detail NotifyAncestor
    
    FocusIn event, serial 36, synthetic NO, window 0x1800001,
        mode NotifyUngrab, detail NotifyAncestor
    
    KeymapNotify event, serial 36, synthetic NO, window 0x0,
        keys:  0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0
               0   0   0   0   0   0   0   0   0   0   0   0   0   0   0   0

The Focus{In,Out}events have a mode of Notify{Grab,Ungrab}. This indicates that a key was handled by another process (probably a shortcut/keybinding application).
If you are using a desktop environment they probably have a keybinding system. In order to see these events is xev, you will need to stop/disable the other program.
If you can't determine what program is stealing the key events the best solution is to start another X session without it running. Press `<Ctrl><Alt><F2>` to tty2, run the following command to start another X session. You can of course change the terminal to whatever you prefer or have installed on your system.

    startx /usr/bin/xterm
Then run xev again. That should give you the result without it being captured by other programs. Note that the window manager that gets started is hover-focus, so you will have to place your cursor above the xev window in order for the keys to be captured.

