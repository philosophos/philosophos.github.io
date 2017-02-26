BASH(1)                     General Commands Manual                    BASH(1)

Readline Command Names
===============================================================================
下面列出的是命令的名称以及默认情况下它们关联的按键序列。
命令名称如果没有对应的按键序列，那么默认是没有关联的。在下列描述中，点 (point)指当前光标位置，标记(mark)指命令 set-mark 保存的光标位置。point 和 mark 之间的文本被称为范围(region)。

Commands for Moving 移动
-------------------------------------------------------------------------------
    beginning-of-line (C-a) 移动到当前行的开始。
    end-of-line       (C-e) 移动到当前行的结尾。
    forward-char      (C-f) 向前移动一字。
    backward-char     (C-b) 向后移动一字。
    forward-word      (M-f) 向前移动到下一词尾。词由字符(字母和数字)组成。
    backward-word     (M-b) 向后移动到当前或上一词首。
    clear-screen      (C-l) 清屏，当前行移到屏幕顶端。有参数时刷新当前行，不清屏。
    redraw-current-line 刷新当前行。

Commands for Manipulating the History 操纵历史行
-------------------------------------------------------------------------------
    accept-line (Newline, Return)
           接受这一行，不管光标在什么位置。如果行非空，将根据变量 HISTCONTROL
           的状态加入到历史列表中。如果行是修改过的历史行，将恢复该历史行
           到初始状态。
    previous-history       (C-p) 从历史列表中取得前一个命令，从列表中向后移动。
    next-history           (C-n) 从历史列表中取得后一个命令，从列表中向前移动。
    beginning-of-history   (M-<) 移动到历史中的第一行。
    end-of-history         (M->) 移动到历史行的末尾，即当前输入的行的末尾。
    reverse-search-history (C-r) 向后增量搜索，按照需要在历史中向“上”移动。
    forward-search-history (C-s) 向前增量搜索，按照需要在历史中向“下”移动。
    non-incremental-reverse-search-history (M-p)
           向后非增量搜索，查找给出的字符串。
    non-incremental-forward-search-history (M-n)
           向前非增量搜索，查找给出的字符串。
    history-search-forward
           向前非增量搜索历史，查找从当前行首到光标之间的字符串。
    history-search-backward
           向后非增量搜索历史，查找从当前行首到光标之间的字符串。
    yank-nth-arg (M-C-y)
           将前一个命令的第一个参数(通常是上一行的第二个词)插入到光标位置。
           有参数 n 时，将前一个命令的第 n 个词 (前一个命令中的词从 0 开始计数)
           插入到光标位置。负数参数则插入前一个命令倒数第 n 个词。
    yank-last-arg (M-., M-_)
           插入前一个命令的最后一个参数 (上一历史条目的最后一个词)。有参数时，
           行为类似于 yank-nth-arg。后继的 yank-last-arg 调用将从历史列表中
           向后移动，依次将每行的最后一个参数插入。
    shell-expand-line (M-C-e)
           扩展行，像 shell 做的那样。其中包含别名和历史扩展，还有所有的 shell
           词的扩展。参见下面的 HISTORY EXPANSION 中关于历史扩展的描述。
    history-expand-line (M-^)
           在当前行进行历史扩展。
           参见下面的 HISTORY EXPANSION 中关于历史扩展的描述。
    magic-space
           在当前行进行历史扩展，并插入一个空格。
           参见下面的 HISTORY EXPANSION 中关于历史扩展的描述。
    alias-expand-line
           在当前行进行别名扩展，参见上面的 ALIASES 中关于别名扩展的描述。
    history-and-alias-expand-line
           在当前行进行历史和别名扩展。
    insert-last-argument (M-., M-_)
           与 yank-last-arg 同义。
    operate-and-get-next (C-o)
           接受当前行，加以执行，从历史中取出相对当前行的下一行进行编辑。
           任何参数都被忽略。
    edit-and-execute-command (C-xC-e)
           启动一个编辑器，编辑当前命令行，将结果作为 shell 命令运行。Bash
           将依次试着运行 $FCEDIT, $EDITOR, 和 emacs 作为编辑器。

Commands for Changing Text 改变文本
-------------------------------------------------------------------------------
    delete-char (C-d)
           删除光标处的字符。如果光标在行首，行中没有字符，最后一次
           输入的字符没有被关联到 delete-char，将返回 EOF.
    backward-delete-char (Rubout)
           删除光标之后的字符。当给出一个数值的参数时，保存删除的文本到 kill ring 中。
    forward-backward-delete-char
           删除光标下的字符，除非光标在行尾，此时删除光标后的字符。
    quoted-insert (C-q, C-v)
           将输入的下一字符保持原样添加到行中。例如，可以用它来插入类似 C-q 的字符。
    tab-insert (C-v TAB)
           插入一个跳格符号。
    self-insert (a, b, A, 1, !, ...)
           插入键入的字符。
    transpose-chars (C-t)
           将光标前的字符向前移动，越过光标处的字符，同时改变光标位置。
           如果光标在行尾，将调换光标之前的两个字符。
    transpose-words (M-t)
           将光标前的词向前移动，越过光标处的词，同时改变光标位置。
           如果光标在行尾，将调换行中的最后两个词。
    upcase-word (M-u)
           将当前(或下一个)词变成全大写。
           有负值的参数时，将前一个词变为大写，但是不移动光标。
    lowercase-word (M-l)
           将当前(或下一个)词变成全小写。
           有负值的参数时，将前一个词变为小写，但是不移动光标。
    capitalize-word (M-c)
           将当前(或下一个)词变为首字大写。
           有负值的参数时，将前一个词变为首字大写，但是不移动光标。
    overwrite-mode
           控制插入/改写模式。给出一个正整数参数时，切换为改写模式。
           给出一个非正数参数时，切换为插入模式。这个命令只影响 emacs 模式；
           vi 模式的改写与此不同。每个对 readline() 的调用都以插入模式开始。
           在改写模式下，关联到 self-insert 的字符替换光标处的字符，
           而不是将它推到右边。关联到 backward-delete-char 的字符以空格替换
           光标前的字符。默认情况下，这个命令没有关联。

Killing and Yanking 剪切和粘贴
-------------------------------------------------------------------------------
    kill-line (C-k)
           剪切从光标到行尾的文本。
    backward-kill-line (C-x Rubout)
           剪切从光标到行首的文本。
    unix-line-discard (C-u)
           剪切从光标到行首的文本。与 backward-kill-line 没有什么区别。
           剪切的文本被保存于 kill-ring 中。
    kill-whole-line
           剪切当前行中所有字符，不管光标在什么位置。
    kill-word (M-d)
           剪切从光标到当前词尾，如果光标在词之间，那么剪切到下一词尾。
    backward-kill-word (M-Rubout)
           剪切光标之后的词。词的边界与 backward-word 使用的相同。
    unix-word-rubout (C-w)
           剪切光标之后的词，使用空白作为词的边界。
           剪切的文本被保存于 kill-ring 中。
    delete-horizontal-space (M-\)
           删除光标两边的所有空格和跳格。
    kill-region
           剪切当前 region 的文本。
    copy-region-as-kill
           将 region 的文本复制到剪切缓冲区中。
    copy-backward-word
           将光标前面的词复制到剪切缓冲区中。词的边界与 backward-word 使用的相同。
    copy-forward-word
           将光标后面的词复制到剪切缓冲区中。词的边界与 backward-word 使用的相同。
    yank (C-y)
           将 kill-ring 顶部的内容粘贴到光标处的缓冲区中
    yank-pop (M-y)
           轮转 kill-ring，粘贴新的顶部内容。只能在 yank 或 yank-pop 之后使用。

Numeric Arguments 数值参数
-------------------------------------------------------------------------------
    digit-argument (M-0, M-1, ..., M--)
           将这个数字加入已有的(already accumulating)参数中，或开始新的参数。
           M-- 开始一个否定的参数。
    universal-argument
           这是指定参数的另一种方法。如果这个命令后面跟着一个或多个数字，
           可能还包含前导的负号，这些数字定义了参数。如果命令之后跟随着数字，
           再次执行 universal-argument 将结束数字参数，但是其他情况下被忽略。
           有一种特殊情况，如果命令后紧接着一个并非数字或负号的字符，下一命令
           的参数计数将乘以 4。参数计数初始是 1，因此第一次执行这个函数，
           使得参数计数为 4，第二次执行使得参数计数为 16，以此类推。

Completing 补全
-------------------------------------------------------------------------------
    complete (TAB)
           试着对光标之前的文本进行补全。Bash 依次试着将文本作为一个变量
           (如果文本以 $ 开始)，一个用户名(如果文本以 ~ 开始)，主机名
           (如果文本以  @  开始)，或者命令(以及别名和函数) 来补全。
           如果这些都没有匹配，将尝试文件名补全。
    possible-completions (M-?)
           列出光标之前的文本可能的补全。
    insert-completions (M-*)
           插入 possible-completions 已产生的光标之前的文本所有的补全。
    menu-complete
           与 complete 相似，但使用可能的补全列表中的某个匹配替换要补全的词。
           重复执行 menu-complete 将遍历可能的补全列表，插入每个匹配。
           到达补全列表的结尾时，鸣终端响铃(按照  bell-style  的设置来做)
           并恢复初始的文本。参数 n 将在匹配列表中向前移动 n 步；负数参数
           可以用于在列表中向后移动。这个命令应当与 TAB 键关联，但默认情况下
           是没有关联的。
    delete-char-or-list
           删除光标下的字符，如果不是在行首或行尾(类似 delete-char)。
           如果在行尾，行为与 possible-completions 一致。这个命令默认没有关联。
    complete-filename (M-/)
           尝试对光标之前的文本进行文件名补全。
    possible-filename-completions (C-x /)
           列出光标之前的文本可能的补全，将它视为文件名。
    complete-username (M-~)
           尝试对光标之前的文本进行补全，将它视为用户名。
    possible-username-completions (C-x ~)
           列出光标之前的文本可能的补全，将它视为用户名。
    complete-variable (M-$)
           尝试对光标之前的文本进行补全，将它视为 shell 变量。
    possible-variable-completions (C-x $)
           列出光标之前的文本可能的补全，将它视为 shell 变量。
    complete-hostname (M-@)
           尝试对光标之前的文本进行补全，将它视为主机名。
    possible-hostname-completions (C-x @)
           列出光标之前的文本可能的补全，将它视为主机名。
    complete-command (M-!)
           尝试对光标之前的文本进行补全，将它视为命令名。命令补全尝试将此
           文本依次与别名，保留字，shell 函数，shell 内建命令，可执行文件名
           进行匹配。
    possible-command-completions (C-x !)
           列出光标之前的文本可能的补全，将它视为命令名。
    dynamic-complete-history (M-TAB)
           尝试对光标之前的文本进行补全，将此文本与历史列表中的行相比较
           来查找可能的补全匹配。
    complete-into-braces (M-{)
           进行文件名补全，将可能的补全列表放在花括号中插入，使得列表可以被
           shell 使用 (参见上面的 Brace Expansion 花括号扩展)。

Keyboard Macros 宏
-------------------------------------------------------------------------------
    start-kbd-macro (C-x ()
           开始保存输入字符为当前键盘宏。
    end-kbd-macro (C-x ))
           停止保存输入字符为当前键盘宏，保存宏定义。
    call-last-kbd-macro (C-x e)
           重新执行上次定义的键盘宏，即显示出宏中的字符，好像它们是从键盘
           输入的一样。

Miscellaneous
-------------------------------------------------------------------------------
    re-read-init-file (C-x C-r)
           读入 inputrc 文件的内容，合并其中的按键关联和变量赋值。
    abort (C-g)
           取消当前编辑命令，鸣终端响铃 (按照 bell-style 的设置来做)。
    do-uppercase-version (M-a, M-b, M-x, ...)
           如果有 Meta 前缀的字符 x 是小写的，那么与命令相关连的是对应的
           大写字符。
    prefix-meta (ESC)
           将输入的下一个字符加上 Meta 前缀。 ESC f 等价于 Meta-f.
    undo (C-_, C-x C-u)
           增量的撤销，分别记住每一行。
    revert-line (M-r)
           撤销这一行的所有修改。这与执行命令 undo 足够多次的效果相同，
           将这一行恢复到初始状态。
    tilde-expand (M-&)
           对当前词进行波浪线扩展。
    set-mark (C-@, M-<space>)
           在 point 处设置 mark。如果给出了数值的参数，标记被设置到那个位置。
    exchange-point-and-mark (C-x C-x)
           交换 point 和 mark。当前光标位置被设置为保存的位置，旧光标位置被
           保存为 mark。
    character-search (C-])
           读入一个字符，光标移动到该字符下次出现的地方。负数将搜索上一个出现。
    character-search-backward (M-C-])
           读入一个字符，光标移动到该字符上次出现的地方。负数将搜索下面的出现。
    insert-comment (M-#)
           没有数值的参数时，readline 变量 comment-begin 的值将被插入到当前
           行首。如果给出一个数值的参数，命令的行为类似于一个开关：如果行首
           字符不匹配 comment-begin 的值，将插入这个值，否则匹配 comment-begin
           的字符将被从行首删除。在两种情况下，这一行都被接受，好像输入了新行符
           一样。comment-begin 的默认值使得这个命令将当前行变成一条 shell
           注释。如果数值参数使得注释字符被删除，这一行将被 shell 执行。
    glob-complete-word (M-g)
          光标之前的词被当作路径扩展的一个模式，尾部暗含了一个星号。
           这个模式被用来为可能的补全产生匹配的文件名列表。
    glob-expand-word (C-x *)
          光标之前的词被当作路径扩展的一个模式，匹配的文件名的列表被插入，
           替换这个词。如果给出一个数值参数，在路径扩展之前将添加一个星号。
    glob-list-expansions (C-x g)
           显示 glob-expand-word 可能产生的扩展的列表，重绘当前行。如果给出
           一个数值参数，在路径扩展之前将添加一个星号。
    dump-functions
           向 readline 输出流打印所有的函数和它们的按键关联。如果给出一个数值
           参数，输出将被格式化，可以用作 inputrc 文件一部分。
    dump-variables
           向 readline 输出流打印所有可设置的 readline 函数。如果给出一个数值
           参数，输出将被格式化，可以用作 inputrc 文件一部分。
    dump-macros
           向 readline 输出流打印所有关联到宏的 readline 按键序列以及它们输出
           的字符串。如果给出一个数值参数，输出将被格式化，可以用作 inputrc
           文件一部分。
    display-shell-version (C-x C-v)
           显示当前 bash 实例的版本信息。

GNU Bash-2.05b                   2002 July 15                          BASH(1)
