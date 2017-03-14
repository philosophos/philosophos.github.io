---
layout: post
title: bash shortkeys
date: 2017-02-22 00:03:32
updated: 2017-02-22 13:03:32
copyright: true
description: Linux | Bash | Shortkeys
categories:
- Linux
- Bash
tags:
- Linux
- Bash
- shortkeys
---

set emacs-mode : `set -o emacs`(default)
set vi-mode    : `set -o vi`

- Moving 移动
- Killing & Yanking 剪切&粘贴
- Delete & Undo 删除&撤销
- Transpose 替换
- Manipulating the History 操纵历史
- Completing 补全
- Other 其它
<!-- more -->

-------------------------------------------------------------------------------
Moving 移动
-------------------------------------------------------------------------------

    beginning-of-line (C-a) 移动到当前行的开始。
    end-of-line       (C-e) 移动到当前行的结尾。
    forward-char      (C-f) 向前移动一个字符。
    backward-char     (C-b) 向后移动一个字符。
    forward-word      (M-f) 向前移动到下一词尾。
    backward-word     (M-b) 向后移动到当前或上一词首。
                      (C-left ) 移动到前一个单词开头
                      (C-right) 移动到后一个单词结尾
                      (C-x) 在上次光标所在字符和当前光标所在字符之间跳转
    character-search            (C-])
           读入一个字符，光标移动到该字符下次出现的地方。负数将搜索上一个出现。
    character-search-backward   (M-C-])
           读入一个字符，光标移动到该字符上次出现的地方。负数将搜索下一个出现。
-------------------------------------------------------------------------------
Killing & Yanking 剪切&粘贴
-------------------------------------------------------------------------------

    kill-line         (C-k) 剪切从光标到行尾的文本
    unix-line-discard (C-u) 剪切从光标到行首的文本
    kill-word         (M-d) 剪切从光标到当前词尾(若光标在词之间，剪切到下一词尾)
    unix-word-rubout  (C-w) 剪切从光标到当前词首(若光标在词之间，剪切到上一词首)
    (M-Backspace)=(C-w)
    yank              (C-y) 粘贴上次剪贴内容
-------------------------------------------------------------------------------
Delete & Undo 删除&撤销
-------------------------------------------------------------------------------

    delete-char             (C-d    ) 删除光标处的字符
    Backspace=              (C-h    ) 删除光标前的字符
    delete-horizontal-space (M-\    ) 删除光标两边的所有空格和跳格。
    quoted-insert           (C-q,C-v) 将输入的下一字符保持原样添加到行中
                                      例如(C-v TAB) 插入一个跳格符号。
    undo        (C-_,C-x C-u) 增量撤销，分别记住每一行。
    revert-line (M-r        ) 撤销所有修改。相当于 undo 足够多次
-------------------------------------------------------------------------------
Transpose 替换
-------------------------------------------------------------------------------

    transpose-chars (C-t) 交换光标处字符与前一个字符
    transpose-words (M-t) 交换光标处单词与前一个单词
    upcase-word     (M-u) 将当前(或下一个)词变成全大写。
    lowercase-word  (M-l) 将当前(或下一个)词变成全小写。
    capitalize-word (M-c) 将当前(或下一个)词变为首字母大写。
-------------------------------------------------------------------------------
Manipulating the History 操纵历史
-------------------------------------------------------------------------------

    previous-history       (C-p) 从历史列表中取得前一个命令，也可用↑键
    next-history           (C-n) 从历史列表中取得后一个命令，也可用↓键
    beginning-of-history   (M-<) 移动到历史中的第一行
    end-of-history         (M->) 移动到历史行的末尾，即当前输入的行的末尾
    reverse-search-history (C-r) 向后增量搜索
    forward-search-history (C-s) 向前增量搜索
    non-incremental-reverse-search-history (M-p) 向后非增量搜索
    non-incremental-forward-search-history (M-n) 向前非增量搜索
    yank-first-arg ( M-C-y ) 插入前一个命令的第一个参数(!^)
    yank-last-arg  ( M-.   ) 插入前一个命令的最后一个参数(!$)
`^oldstr^newstr` 替换前一次命令中字符串

-------------------------------------------------------------------------------
Completing 补全
-------------------------------------------------------------------------------

    complete (TAB) 试着对光标之前的文本进行补全。
           Bash 依次试着将文本作为
           变量名(如果文本以 $ 开始)，
           用户名(如果文本以 ~ 开始)，
           主机名(如果文本以 @ 开始)，
           命令名(以及别名和函数)来补全,
           如果这些都没有匹配，将尝试文件名补全。

    complete-filename (M-/) 尝试对光标前的文本进行文件名补全
    complete-username (M-~) 尝试对光标前的文本进行补全，将它视为用户名
    complete-variable (M-$) 尝试对光标前的文本进行补全，将它视为shell变量
    complete-hostname (M-@) 尝试对光标前的文本进行补全，将它视为主机名
    complete-command  (M-!) 尝试对光标前的文本进行补全，将它视为命令名
    possible-completions          (M-?  ) 列出光标前的文本可能的补全。
    possible-filename-completions (C-x /) 列出光标前的文本可能的补全，将它视为文件名
    possible-username-completions (C-x ~) 列出光标前的文本可能的补全，将它视为用户名
    possible-variable-completions (C-x $) 列出光标前的文本可能的补全，将它视为shell变量
    possible-hostname-completions (C-x @) 列出光标前的文本可能的补全，将它视为主机名
    possible-command-completions  (C-x !) 列出光标前的文本可能的补全，将它视为命令名
-------------------------------------------------------------------------------
Other 其它
-------------------------------------------------------------------------------

    C-s 锁住终端
    C-q 解锁终端
    C-l 清屏;相当于命令`clear`
    C-c 终止当前进程，另起一行
    C-z 把当前进程放到后台
    C-d 在没有输入的空行，相当于`exit`

-------------------------------------------------------------------------------
Extending Reading
-------------------------------------------------------------------------------
[man.en.bash.shortkeys](/man/man-en.bash.shortkeys.html)
[man.zh.bash.shortkeys](/man/man-zh.bash.shortkeys.html)
