---
layout: post
title: zsh shortkeys
copyright: true
date: 2017-04-08 19:47:17
updated:
description:
categories:
- Linux
- Shell
- Zsh
tags:
- Linux
- Shell
- Zsh
- shortkeys
---

zsh command line editor
- Movement 移动
- History Control 操纵历史
- Modifying Text 修改文本
- Arguments 参数
- Completion 补全
- Miscellaneous 其他

For more info, see [man zshzle](https://linux.die.net/man/1/zshzle)
<!-- more -->

-------------------
Movement 移动
-------------------

    backward-char (^B ESC-[D)  
    backward-word (ESC-B ESC-b)  
    beginning-of-line (^A)  
    end-of-line (^E)  
    forward-char (^F ESC-[C)  
    vi-find-next-char (^X^F)
    forward-word (ESC-F ESC-f)  
    vi-goto-column (ESC-|)

-------------------
History Control 操纵历史
-------------------

    beginning-of-buffer-or-history (ESC-<)
    down-line-or-history (^N ESC-[B)
    end-of-buffer-or-history (ESC->)  
    history-incremental-search-backward (^R ^Xr)  
    history-incremental-search-forward (^S ^Xs)  
    history-search-backward (ESC-P ESC-p)  
    history-search-forward (ESC-N ESC-n)  
    infer-next-history (^X^N)  
    insert-last-word (ESC-_ ESC-.)  
    up-line-or-history (^P ESC-[A)

-------------------
Modifying Text 修改文本
-------------------

    backward-delete-char (^H ^?)  
    backward-kill-line
    backward-kill-word (^W ESC-^H ESC-^?)  
    capitalize-word (ESC-C ESC-c)  
    copy-region-as-kill (ESC-W ESC-w)  
    copy-prev-word (ESC-^_)  
    down-case-word (ESC-L ESC-l)  
    kill-word (ESC-D ESC-d)  
    vi-join (^X^J)
    kill-line (^K)  
    kill-buffer (^X^K)  
    kill-whole-line (^U)  
    vi-match-bracket (^X^B)
    overwrite-mode (^X^O)  
    quoted-insert (^V)  
    quote-line (ESC-')  
    quote-region (ESC-")  
    self-insert (printable characters)  (printable characters and some control characters)
    self-insert-unmeta (ESC-^I ESC-^J ESC-^M)  
    transpose-chars (^T)  
    transpose-words (ESC-T ESC-t)  
    up-case-word (ESC-U ESC-u)  
    yank (^Y)  
    yank-pop (ESC-y)  

-------------------
Arguments 参数
-------------------

    digit-argument (ESC-0..ESC-9) (1-9) 
    neg-argument (ESC--)  

-------------------
Completion 补全
-------------------

    delete-char-or-list (^D)  
    expand-or-complete (TAB)
    expand-history (ESC-space ESC-!)  
    expand-word (^X*)  
    list-choices (ESC-^D)
    list-expand (^Xg ^XG)

-------------------
Miscellaneous 其他
-------------------

    accept-and-hold (ESC-A ESC-a)  
    accept-line (^J ^M)
    accept-line-and-down-history (^O)  
    vi-cmd-mode (^X^V)
    clear-screen (^L ESC-^L)
    exchange-point-and-mark (^X^X)  
    execute-named-cmd (ESC-x)
    execute-last-named-cmd (ESC-z)  
    get-line (ESC-G ESC-g)  
    push-line (^Q ESC-Q ESC-q)  
    send-break (^G ESC-^G)  
    run-help (ESC-H ESC-h)  
    set-mark-command (^@)  
    spell-word (ESC-$ ESC-S ESC-s)  
    undo (^_ ^Xu ^X^U)
    what-cursor-position (^X=)
