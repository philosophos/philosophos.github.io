---
layout: post
title: esc-seq and tput (1)--colors
date: 2017-02-19 20:52:35
updated: 2017-02-21
description: Linux | tput | esc-seq
categories: Linux
tags:
- Linux
- tput
- esc-seq
---
===============================================================================
**Use Escape Sequence To Set Colors**
    Nowadays terminal like XTerm & URxvt support 256color. But only the first 16 can be specified using resources currently or other configuration file, the rest can only be shown via command sequences ("escape codes").  
    The ECMA-48 SGR sequence ( ESC \[ parameters m ) can set foreground & background colors.  
**Use 'tput' To Set Colors**
    It  is  generally not good practice to hard-wire terminal controls into programs.  Linux supports a terminfo(5) database of terminal  capabilities.   Rather than emitting console escape sequences by hand, you will almost always want to use a terminfo-aware screen  library  or  utility such as tput(1).
    `tput` command can initialize a terminal or query terminfo database,and could be used in shell conveniently.
**Use Escape Sequence to Set Display Attributes**
    Apart from display foreground & background,the ECMA-48 SGR sequence ( ESC \[ parameters m ) can set display attributes.
**Use 'tput' to Set Display Attributes**
**Extensive Reading: about setaf/setab & setf/setb**
    From man terminfo
    section Color Handling
<!-- more -->

-------------------------------------------------------------------------------
Use Escape Sequence To Set Colors
-------------------------------------------------------------------------------
Nowadays terminal like XTerm & URxvt support 256color. But only the first 16 can be specified using resources currently or other configuration file, the rest can only be shown via command sequences ("escape codes").  
The ECMA-48 SGR sequence ( ESC \[ parameters m ) can set foreground & background colors.  

    Parameters For color0 ~ color7:
        30  set black foreground    40  set black background
        31  set red foreground      41  set red background
        32  set green foreground    42  set green background
        33  set brown foreground    43  set brown background
        34  set blue foreground     44  set blue background
        35  set magenta foreground  45  set magenta background
        36  set cyan foreground     46  set cyan background
        37  set white foreground    47  set white background
    for color8 ~ color15,the parameters 90~97 selected the foreground color,
    while 100~107 selected the background.(Not Supported On Some Terminals)

**For example:**
&emsp;`echo -en '\e[30;42mBrighten You Terminal'`  
It will print the sentence in green foreground and black background.  
Apart from '\e','ESC' could be written in octal or hexadecimal (e.g.,'\033' or '\x1b'). And `printf`(shell builtin) can be used instead of `echo -en`.

To set the foreground/background with color256:
&emsp;`printf '\e[38;5;<256-color-code>m'`
&emsp;`printf '\e[48;5;<256-color-code>m'`
`<256-color-code>`from 0 to 255.
color16 ~ color231 is 6x6x6=216 colors;
color232 ~ color255 is 24 grayscale colors from black to white.

Just know above,you can make your terminal colorful,like bash prompt `PS1`&`PS2`.

-------------------------------------------------------------------------------
Use 'tput' To Set Colors
-------------------------------------------------------------------------------

    From man console_codes
    It  is  generally not good practice to hard-wire terminal controls into
    programs.  Linux supports a terminfo(5) database of terminal  capabili‐
    ties.   Rather than emitting console escape sequences by hand, you will
    almost always want to use a terminfo-aware screen  library  or  utility
    such as ncurses(3), tput(1), or reset(1).

`tput` command can initialize a terminal or query terminfo database,and could be used in shell conveniently.

To query the terminal support colors' types:  
&emsp;`tput colors`  
it will output a number like 8,16,88,256.  

To change the current foreground/background color on a terminal:  
&emsp;`tput setf <256-color-code>`
&emsp;`tput setb <256-color-code>`
&emsp;`tput setaf <256-color-code>`
&emsp;`tput setab <256-color-code>`
**NOTE**:Only `tput setab/setaf <256-color-code>` can work in XTerm.  
`tput setab/setaf 1` show red,`tput setab/setaf 4` show blue,and `tput setb/setf 1/4` have the reverse effect.
Be careful to not confuse the two sets of color capabilities; otherwise red/blue will be interchanged on the display.

**For example:**  
&emsp;`tput setaf 255;tput setaf 231;echo 'Brighten You Terminal'`  
It will print the sentence in color255 as foreground and color231 as background.  
  
-----------------------------------------------------------
**Use Escape Sequence to Set Display Attributes**
-----------------------------------------------------------
Apart from display foreground & background,the ECMA-48 SGR sequence ( ESC \[ parameters m ) can set display attributes.

    param   result
    0       reset all attributes to their defaults
    1       set bold
    21      set normal intensity (ECMA-48 says "doubly underlined")
    2       set half-bright (simulated with color on a color display)
    22      set normal intensity
    4       set underscore (simulated with color on a color  display)
            (the  colors  used  to  simulate dim or underline are set
            using ESC ] ...)
    24      underline off
    5       set blink
    25      blink off
    7       set reverse video
    27      reverse video off

-----------------------------------------------------------
**Use 'tput' to Set Display Attributes**
-----------------------------------------------------------
Some terminfo string capabilities about display attributes.

    Variable String       Capname  TCapCode  Description
    enter_blink_mode      blink    mb        turn on blinking
    enter_bold_mode       bold     md        turn on bold mode
    enter_dim_mode        dim      mh        turn on half-bright mode
    enter_italics_mode    sitm     ZH        Enter italic mode
    exit_italics_mode     ritm     ZR        End italic mode
    enter_reverse_mode    rev      mr        turn on reverse video mode
    enter_underline_mode  smul     us        begin underline mode
    exit_underline_mode   rmul     ue        exit underline mode
    exit_attribute_mode   sgr0     me        turn off all attributes

-----------------------------------------------------------
**For example**
`tput bold;printf "mBOLD\n`" will have same output with `printf "\e[1mBOLD\n"`.
`tput sgr0` will have the same affect with `printf "\e[0m"`.

-------------------------------------------------------------------------------
Extensive Reading: about setaf/setab & setf/setb
-------------------------------------------------------------------------------
[man-terminfo.Color Handling](/man/man-en.terminfo.color.html)
