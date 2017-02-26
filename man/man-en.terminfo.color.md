From man terminfo
section Color Handling
=============================

While the curses library works with color pairs (reflecting the inability of some devices to set foreground and  background  colors  independently), there are separate capabilities for setting these features:

-   To  change  the  current  foreground  or background color on a Tek‐
    tronix-type terminal, use setaf (set  ANSI  foreground)  and  setab
    (set  ANSI background) or setf (set foreground) and setb (set back‐
    ground).  These take one parameter, the  color  number.   The  SVr4
    documentation  describes only setaf/setab; the XPG4 draft says that
    "If the terminal supports ANSI escape sequences to  set  background
    and  foreground,  they  should be coded as setaf and setab, respec‐
    tively.

-   If the terminal supports other escape sequences to  set  background
    and  foreground,  they  should  be  coded as setf and setb, respec‐
    tively.  The vidputs and the refresh functions use  the  setaf  and
    setab capabilities if they are defined.

The  setaf/setab and setf/setb capabilities take a single numeric argu‐
ment each.  Argument values 0-7 of setaf/setab are portably defined  as
follows  (the  middle  column  is the symbolic #define available in the
header for the curses or ncurses libraries).  The terminal hardware  is
free to map these as it likes, but the RGB values indicate normal loca‐
tions in color space.

             Color       #define       Value       RGB
             black     COLOR_BLACK       0     0, 0, 0
             red       COLOR_RED         1     max,0,0
             green     COLOR_GREEN       2     0,max,0
             yellow    COLOR_YELLOW      3     max,max,0
             blue      COLOR_BLUE        4     0,0,max
             magenta   COLOR_MAGENTA     5     max,0,max
             cyan      COLOR_CYAN        6     0,max,max
             white     COLOR_WHITE       7     max,max,max

The argument values of setf/setb historically correspond to a different mapping, i.e.,

             Color       #define       Value       RGB
             black     COLOR_BLACK       0     0, 0, 0
             blue      COLOR_BLUE        1     0,0,max
             green     COLOR_GREEN       2     0,max,0
             cyan      COLOR_CYAN        3     0,max,max
             red       COLOR_RED         4     max,0,0
             magenta   COLOR_MAGENTA     5     max,0,max
             yellow    COLOR_YELLOW      6     max,max,0
             white     COLOR_WHITE       7     max,max,max

It is important to not confuse the two sets of color capabilities; otherwise red/blue will be interchanged on the display.
