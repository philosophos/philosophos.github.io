---
layout: post
title: esc-seq and tput (2)--cursor
date: 2017-02-21 03:26:52
updated: 2017-02-22 22:26:52
description: Linux | tput | esc-seq
categories: Linux
tags: 
- Linux
- tput
- esc-seq
---
===========================================================
**Use Escape Sequence To Move Cursor**
**Use 'tput' To Move Cursor**
- Indicated Row, Column
- Up,Down,Right,Left
- Save & Restore Cursor location
- Make Cursor Invisible/Visible
- Other
<!-- more -->

-----------------------------------------------------------
Use Escape Sequence To Move Cursor
-----------------------------------------------------------
ECMA-48 CSI sequences

    CSI (or ESC [) is followed by a sequence of parameters,  at  most  NPAR
    (16),  that  are  decimal numbers separated by semicolons.  An empty or
    absent parameter is taken to be 0.  The sequence of parameters  may  be
    preceded by a single question mark.
    
    However,  after  CSI [ (or ESC [ [) a single character is read and this
    entire sequence is ignored.  (The idea is to ignore an echoed  function
    key.)
    
    The action of a CSI sequence is determined by its final character.

    A   CUU       Move cursor up the indicated # of rows.
    B   CUD       Move cursor down the indicated # of rows.
    e   VPR       Move cursor down the indicated # of rows.
    a   HPR       Move cursor right the indicated # of columns.
    C   CUF       Move cursor right the indicated # of columns.
    D   CUB       Move cursor left the indicated # of columns.

    E   CNL       Move cursor down the indicated # of rows, to column 1.
    F   CPL       Move cursor up the indicated # of rows, to column 1.
    G   CHA       Move cursor to indicated column in current row.
    `   HPA       Move cursor to indicated column in current row.
    d   VPA       Move cursor to the indicated row, current column.
    H   CUP       Move cursor to the indicated row, column (origin at 1,1).
    f   HVP       Move cursor to the indicated row, column.
    s   ?         Save cursor location.
    u   ?         Restore cursor location.

Control characters

    Oct   Dec   Hex   Char
    010   8     08    BS  '\b' (backspace)
    011   9     09    HT  '\t' (horizontal tab)
    012   10    0A    LF  '\n' (new line)
    013   11    0B    VT  '\v' (vertical tab)
    014   12    0C    FF  '\f' (form feed)
    015   13    0D    CR  '\r' (carriage ret)

ESC- but not CSI-sequences

    ESC c     RIS      Reset.
    ESC D     IND      Linefeed.
    ESC E     NEL      Newline.
    ESC M     RI       Reverse linefeed.

-----------------------------------------------------------
Use 'tput' To Move Cursor
-----------------------------------------------------------

    Variable String   Capname  TCapCode  Description
    
    clear_screen        clear     cl     clear screen and home cursor (P*)
    carriage_return     cr        cr     carriage return (P*)
    newline             nel       nw     newline (behave like cr followed by lf)
    cursor_home         home      ho     home cursor (if no cup)
                                  
    column_address      hpa       ch     horizontal position #1, absolute (P)
    row_address         vpa       cv     vertical position #1 absolute (P)
    cursor_address      cup       cm     move to row #1 columns #2
    cursor_mem_address  mrcup     CM     memory relative cursor addressing, move to row #1 columns #2
    enter_ca_mode       smcup     ti     string to start programs using cup
    exit_ca_mode        rmcup     te     strings to end programs using cup
    non_rev_rmcup       nrrmc     NR     smcup does not reverse rmcup
    cursor_to_ll        ll        ll     last line, first column (if no cup)
                                  
    cursor_up           cuu1      up     up one line
    cursor_down         cud1      do     down one line
    cursor_left         cub1      le     move left one space
    cursor_right        cuf1      nd     move right one space
    parm_up_cursor      cuu       UP     up #1 lines (P*)
    parm_down_cursor    cud       DO     down #1 lines (P*)
    parm_left_cursor    cub       LE     move #1 characters to the left (P)
    parm_right_cursor   cuf       RI     move #1 characters to the right (P)
                                  
    restore_cursor      rc        rc     restore cursor to position of last save_cursor
    save_cursor         sc        sc     save current cursor position (P)
    cursor_invisible    civis     vi     make cursor invisible
    cursor_visible      cvvis     vs     make cursor very visible
    cursor_normal       cnorm     ve     make cursor appear normal (undo civis/cvvis)

-----------------------------------------------------------
Indicated Row, Column
-----------------------------------------------------------
**(CUP,HVP)Move cursor to the indicated row, column (origin at 1,1)**
`printf "\e[2;30H"` `tput cup 2 30`
`printf "\e[2;30f"`
(move cursor to 2nd row,30th colume.)
**(HPA,CHA)Move cursor to indicated column in current row.**
`\printf "\e[5G"` `tput hpa 5`
(move cursor to 5th column in current row)
**(VPA)Move cursor to indicated row in current column.**
`printf "\e[5d"` `tput vpa 5`
(move cursor to 5th row in current column)

-----------------------------------------------------------
Up,Down,Right,Left
-----------------------------------------------------------
**(CUU,RI)Move cursor up**
`printf "\e[A"` `printf "\eM"`  `tput cuu1`
`printf "\e[1A"`                `tput cuu 1`
`printf "\e[2A"`                `tput cuu 2`
**(VT,FF,IND)Move cursor down**
`printf "\v"` `printf "\f"` `printf "\eD"`
**(CUD,VPR)Move cursor down(invalid in bottom line)**
`printf "\e[B"`  `printf "\e[e"`
`printf "\e[1B"` `printf "\e[1e"`   `tput cud 1`
`printf "\e[2B"` `printf "\e[2e"`   `tput cud 2`
**(CUF,HPR)Move cursor right**
`printf "\e[C"`  `printf "\e[a"`    `tput cuf1`
`printf "\e[1C"` `printf "\e[1a"`   `tput cuf 1`
`printf "\e[2C"` `printf "\e[2a"`   `tput cuf 2`
**(CUB)Move cursor left**
`printf "\e[D"`     `tput cub1`
`printf "\e[1D"`    `tput cub 1`
`printf "\e[2D"`    `tput cub 2`

-----------------------------------------------------------
Save & Restore Cursor location
-----------------------------------------------------------
**Save cursor location**    ` printf "\e[s" `  ` tput sc `
**Restore cursor location** ` printf "\e[u" `  ` tput rc `

-----------------------------------------------------------
Make Cursor Invisible/Visible
-----------------------------------------------------------
**Make Cursor Invisible** ` tput civis `
**Make Cursor Visible**   ` tput cvvis `
**undo civis/cvvis**      ` tput cnorm `

-----------------------------------------------------------
Other
-----------------------------------------------------------
**(BS)Backspace**
`printf "\b"` <Backspace> <C-h>
**(CR)Carriage Return**
`printf '\r'` `tput cr` <C-a>
**(LF,NEL)New Line**
`printf "\n"` `printf "\eE"` `tput cud1` <Enter>
**clear**
`printf '\ec'` `tput clear` `clear` <C-l>

-----------------------------------------------------------
