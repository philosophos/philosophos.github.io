---
layout: post
title: Password Protection of GRUB Menu
copyright: true
date: 2017-03-04 21:26:24
updated:
description: set password protection for grub menu
categories: Linux
tags:
- Linux
- GRUB
- password
---

By default, the boot loader interface is accessible to anyone with physical access to the console: anyone can select and edit any menu entry, and anyone can get direct access to a GRUB shell prompt.  For most systems, this is reasonable since anyone with direct physical access has a variety of other ways to gain full access, and requiring authentication at the boot loader level would only serve to make it difficult to recover broken systems. 

However, in some environments, such as kiosks, it may be appropriate to lock down the boot loader to require authentication before performing certain operations.

- Set GRUB Passwords
- Enable More Authentication Support

<!-- more -->

-----------------------------------------------------------
Set GRUB Passwords
-----------------------------------------------------------
The `password` and `password_pbkdf2` commands can be used to define users, each of which has an associated password. `password` sets the password in plain text, requiring grub.cfg to be secure; `password_pbkdf2` sets the password hashed using the Password-Based Key Derivation Function (RFC 2898), requiring the use of grub-mkpasswd-pbkdf2.

    # grub-mkpasswd-pbkdf2
    Enter password:
    Reenter password:
    PBKDF2 hash of your password is grub.pbkdf2.sha512.10000.C9169FD3A50B9F7ABB1CCEF23E010EFC1EF0191494F9D987D52343D58D48D7DF0DED2C93F225B1F09E473B58F78BE61D4FB8219CD3F429F02DB136C624DF3BE0.0B7315DC33B782922A56DA902C1F412CC15894BEE92AA218BC5ADC6E8B929CBEFA1DE3C768D1B7BF81068386249901BD1868E8F7FE90870F77319B6B8490E409


{% codeblock lang:sh Add the following to /etc/grub.d/00_header %}
cat << EOF

set superusers="root"
password_pbkdf2 root grub.pbkdf2.sha512.10000.C9169FD3A50B9F7ABB1CCEF23E010EFC1EF0191494F9D987D52343D58D48D7DF0DED2C93F225B1F09E473B58F78BE61D4FB8219CD3F429F02DB136C624DF3BE0.0B7315DC33B782922A56DA902C1F412CC15894BEE92AA218BC5ADC6E8B929CBEFA1DE3C768D1B7BF81068386249901BD1868E8F7FE90870F77319B6B8490E409
password user1 password_in_plain_text

EOF
{% endcodeblock %}

run `grub-mkconfig -o /boot/grub/grub.cfg` to regenerate grub configuration file. Your GRUB command line, boot parameters and all boot entries are now protected.

**Warning**  ---  Though grub password was set, ANYONE with direct physical access can still boot from Udisk or PXE via changing BIOS settings and run OS as ROOT, which means you should set BIOS password to protected OS from unauthorized access.

-----------------------------------------------------------
Enable More Authentication Support
-----------------------------------------------------------

In order to enable authentication support, the `superusers` environment variable must be set to a list of usernames, separated by any of spaces, commas, semicolons, pipes, or ampersands. Superusers are permitted to use the GRUB command line, edit menu entries, and execute any menu entry. If `superusers` is set, then use of the command line is automatically restricted to superusers.

Other users may be given access to specific menu entries by giving a list of usernames (as above) using the `--users` option to the `menuentry` command.
If the `--users` option is not used for a menu entry, then that only superusers are able to use it.

Adding `--unrestricted` to a menu entry will
- allow any user to boot the OS
- preventing the user from editing the entry
- preventing access to the grub command console

Only a superuser or users specified with the `--user` switch will be able to edit the menu entry.

**Warning**  ---  Adding `--unrestricted` to a menu entry with `submenuentry` will allow ANYONE gain `superuser` authority in the submenuentry !  So make sure the `--unrestricted` option is not used for a menu entry with `submenuentry`, Or add `GRUB_DISABLE_SUBMENU=y` to `/etc/default/grub` / `/etc/sysconfig/grub` to disable submenu.

{% codeblock lang:sh Putting this together, a typical `grub.cfg` fragment might look like this : %}
menuentry "can be run only by superusers" {
	set root=(hd0,1)
	linux /vmlinuz single
}

menuentry "can be run only by superusers" --users "" {
	set root=(hd0,1)
	linux /vmlinuz single
}

menuentry "can be run by user1 or superusers" --users user1 {
	set root=(hd0,2)
	chainloader +1
}

menuentry "can be run by any user" --unrestricted {
	set root=(hd0,1)
	linux /vmlinuz
}
{% endcodeblock %}

**NOTE**  ---  The grub-mkconfig program does not yet have built-in support for generating configuration files with authentication. You can edit `/boot/grub/grub.cfg` after running `grub-mkconfig -o /boot/grub/grub.cfg`.

