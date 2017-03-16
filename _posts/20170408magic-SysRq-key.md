---
layout: post
title: magic SysRq key
copyright: true
date: 2017-04-08 20:32:52
updated:
description:
categories:
- Linux
tags:
- Linux
- sysrq
- shortkeys
---
It is a 'magical' key combo you can hit which the kernel will respond to regardless of whatever else it is doing, unless it is completely locked up.

- enable the magic SysRq key
- use the magic SysRq key
- the 'command' keys
- Uses

For more info, see [sysrq](https://www.kernel.org/doc/Documentation/admin-guide/sysrq.rst)
<!-- more -->

-----------------------------
enable the magic SysRq key
-----------------------------

You need to say "yes" to 'Magic SysRq key (CONFIG_MAGIC_SYSRQ)' when configuring the kernel.
When running a kernel with SysRq compiled in, `/proc/sys/kernel/sysrq` controls the functions allowed to be invoked via the SysRq key. The default value in this file is set by the `CONFIG_MAGIC_SYSRQ_DEFAULT_ENABLE` config symbol, which itself defaults to 1. Here is the list of possible values in `/proc/sys/kernel/sysrq`:

-  `0` - disable sysrq completely
-  `1` - enable all functions of sysrq
- `>1` - bitmask of allowed sysrq functions (see below for detailed function description)::

          2 = 0x2   - enable control of console logging level
          4 = 0x4   - enable control of keyboard (SAK, unraw)
          8 = 0x8   - enable debugging dumps of processes etc.
         16 = 0x10  - enable sync command
         32 = 0x20  - enable remount read-only
         64 = 0x40  - enable signalling of processes (term, kill, oom-kill)
        128 = 0x80  - allow reboot/poweroff
        256 = 0x100 - allow nicing of all RT tasks

The number may be written here either as decimal or as hexadecimal with the 0x prefix. `CONFIG_MAGIC_SYSRQ_DEFAULT_ENABLE` must always be written in hexadecimal.

Note that the value of `/proc/sys/kernel/sysrq` influences only the invocation via a keyboard. Invocation of any operation via `/proc/sysrq-trigger` is always allowed (by a user with admin privileges).

You can set the value in the file by the following command::

    echo 1 >/proc/sys/kernel/sysrq
or

    sysctl -w kernel.sysrq=1
If you wish to have it enabled during boot, edit /etc/sysctl.d/99-sysctl.conf and insert the text `kernel.sysrq = 1`. 
If you want to make sure it will be enabled even before the partitions are mounted and in the initrd, then add `sysrq_always_enabled=1` to your kernel parameters. 

-----------------------------
use the magic SysRq key
-----------------------------

**On x86**
Press the key combo `<Alt><SysRq><command key>`.

**On PowerPC**
Press `<Alt><Print Screen>(or <F13>)<command key>`, `<Print Screen>(or <F13>)<command key>` may suffice.

**On SPARC**
Press `<Alt><Stop><command key>`

**On the serial console** (PC style standard serial ports only)
Send a ``BREAK``, then within 5 seconds a command key.
Sending ``BREAK`` twice is interpreted as a normal BREAK.

**On all**
write a character to /proc/sysrq-trigger.  e.g.::

    echo t > /proc/sysrq-trigger

**NOTE**
On some devices, notably laptops, the Fn key may need to be pressed to use the magic SysRq key.
Some keyboards may not provide a separate SysRq key. In this case, a separate PrtScr key should be present.
Some keyboards can't handle so many keys being pressed at the same time, so you might have better luck with press `Alt`, press `SysRq`, release `SysRq`, press `<command key>`, release everything.

-----------------------------
the 'command' keys
-----------------------------
The combinations always assume the QWERTY keyboard layout; for example, on a Dvorak Simplified Keyboard, the combination to shut the system down uses the R key instead of O.

    ======= ===================================================================
    Command	    Function
    ======= ===================================================================
    0-9     Sets the console log level, controlling which kernel messages
            will be printed to your console. (``0``, for example would make
            it so that only emergency messages like PANICs or OOPSes would
            make it to your console.)
    b   Will immediately reboot the system without syncing or unmounting
            your disks.
    c   Will perform a system crash by a NULL pointer dereference.
            A crashdump will be taken if configured.
    d   Shows all locks that are held.
    e   Send a SIGTERM to all processes, except for init.
    f   Will call the oom killer to kill a memory hog process, but do not
            panic if nothing can be killed.
    g   Used by kgdb (kernel debugger)
    h   Will display help (actually any other key than those listed
            here will display help. but ``h`` is easy to remember :-)
    i   Send a SIGKILL to all processes, except for init.
    j   Forcibly "Just thaw it" - filesystems frozen by the FIFREEZE ioctl.
    k   Secure Access Key (SAK) Kills all programs on the current virtual
            console. NOTE: See important comments below in SAK section.
    l   Shows a stack backtrace for all active CPUs.
    m   Will dump current memory info to your console.
    n   Used to make RT tasks nice-able
    o   Will shut your system off (if configured and supported).
    p   Will dump the current registers and flags to your console.
    q   Will dump per CPU lists of all armed hrtimers (but NOT regular
            timer_list timers) and detailed information about all
            clockevent devices.
    r   Turns off keyboard raw mode and sets it to XLATE.
    s   Will attempt to sync all mounted filesystems.
    t   Will dump a list of current tasks and their information to your
            console.
    u   Will attempt to remount all mounted filesystems read-only.
    v   Forcefully restores framebuffer console
    v   Causes ETM buffer dump [ARM-specific]
    w   Dumps tasks that are in uninterruptable (blocked) state.
    x   Used by xmon interface on ppc/powerpc platforms.
            Show global PMU Registers on sparc64.
            Dump all TLB entries on MIPS.
    y   Show global CPU Registers [SPARC-64 specific]
    z   Dump the ftrace buffer

-----------------------------
Uses
-----------------------------
There are several low level shortcuts that are implemented in the kernel which can be used for debugging and recovering from an unresponsive system. Whenever possible, it is recommended that you use these shortcuts instead of doing a hard shutdown (holding down the power button to completely power off the system).
A common use of the magic SysRq key is to perform a safe reboot of a Linux computer which has otherwise locked up (abbr. REISUB). This can prevent a fsck being required on reboot and gives some programs a chance to save emergency backups of unsaved work. The QWERTY (or AZERTY) mnemonics: "Raising Elephants Is So Utterly Boring", "Reboot Even If System Utterly Broken" or simply the word "BUSIER" read backwards, are often used to remember the following SysRq-keys sequence:

    unRaw      (take control of keyboard back from X),
     tErminate (send SIGTERM to all processes, allowing them to terminate gracefully),
     kIll      (send SIGKILL to all processes, forcing them to terminate immediately),
      Sync     (flush data to disk),
      Unmount  (remount all filesystems read-only),
    reBoot.
When magic SysRq keys are used to kill a frozen graphical program, the program has no chance to restore text mode. This can make everything unreadable. The commands textmode (part of SVGAlib) and the reset command can restore text mode and make the console readable again.

On distributions that do not include a textmode executable, the key command Ctrl+Alt+F1 may sometimes be able to force a return to a text console. (Use F1, F2, F3,..., F(n), where n is the highest number of text consoles set up by the distribution. Ctrl+Alt+F(n+1) would normally be used to reenter GUI mode on a system on which the X server has not crashed.)
