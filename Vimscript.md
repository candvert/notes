```

:set conceallevel=0 显示链接标志
:hi link HelpBar Normal 链接
:hi link HelpStar Normal 链接

复制示例配置到vimrc，Linux上
:!cp -i $VIMRUNTIME/vimrc_example.vim ~/.vimrc
Windows上
:!copy $VIMRUNTIME/vimrc_example.vim $VIM/_vimrc
或:!copy $VIMRUNTIME/vimrc_example.vim $HOME/_vimrc


Ctrl + O 跳转上一个
Ctrl + I 跳转下一个
`` 跳转上一个
'' 跳转上一个
:jumps 显示可以跳转的位置
ma 标记光标处的位置，m表示mark，a为26字符之一
`a 跳转到标记a的位置
'a 跳转到标记a所在行的行首
:marks 列出标记
'" 跳转最后编辑该文件的位置
'[ 上个更改的开头
'] 上个更改的结尾



   system vimrc file: "/etc/vimrc"
     user vimrc file: "$HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
 3rd user vimrc file: "~/.config/vim/vimrc"
      user exrc file: "$HOME/.exrc"
       defaults file: "$VIMRUNTIME/defaults.vim"
  fall-back for $VIM: "/etc"
 f-b for $VIMRUNTIME: "/usr/share/vim/vim91"


vimrc可以包含所有“: xxx”的命令
undofile选项，退出vim再打开文件也可以undo，代价是存储一个额外文件


:map <F5> i{<Esc>ea}<Esc>
i{<Esc>ea}<Esc>表示i{进入插入模式并输入{，<Esc>退出插入模式，e移动到单词末尾，a}
附加}在单词末尾，<Esc>退出编辑模式

\可以作为leader key
:map 列出按键映射

启用matchit插件，在vimrc加入该句
packadd! matchit

插件就是一个vim script文件
存在两种插件：
全局插件，对所有文件生效
filetype插件，只对特定类型文件生效
查看自动加载的插件，:h standard-plugin-list
插件所在目录（Windows）：$HOME/vimfiles/plugin or $VIM/vimfiles/plugin

:options   列出选项和one-line解释
:set option& 将选项还原为默认值
whichwrap选项 可以设置在到达文件尾或文件头时仍可以继续移动

:split 分隔窗口，新窗格文件为当前文件，光标留在上面的窗格
:split two.c 分隔窗口，新窗格文件为two.c
:new 分隔窗口，新窗格文件为空
:close 关闭一个窗格（:q和ZZ也可以，但是:close可以防止你在只有一个窗格的情况下退出vim）
:only 关闭所有其他窗格
:3split two.c 新窗格的高度为3行
Ctrl + w后+     窗格高度增加一行
Ctrl + w后-     窗格高度减少一行
4Ctrl + w后+     窗格高度增加4行
24Ctrl + w后_     设置窗格高度为24行

:vsplit 竖向分隔窗口
:vnew 分隔窗口，新窗格文件为空

winminheight选项 设置窗格最小高度
winminwidth选项 设置窗格最小宽度

在窗格间移动
CTRL-W h        move to the window on the left
CTRL-W j        move to the window below
CTRL-W k        move to the window above
CTRL-W l        move to the window on the right

CTRL-W t        move to the TOP window
CTRL-W b        move to the BOTTOM window

移动窗格
CTRL-W K        move window to the top
CTRL-W H        move window to the far left
CTRL-W J        move window to the bottom
CTRL-W L        move window to the far right

:qall 关闭所有窗格
:wall
:wqall
:qall!


vim -o one.txt two.txt three.txt 打开三个窗格
vim -O one.txt two.txt three.txt 打开三个窗格，纵向分隔

在shell中使用vimdiff查看两个文件的不同之处：
vimdiff a.txt b.txt
]c 跳转下一个不同之处
[c 跳转上一个不同之处
4]c
:diffupdate
dp 光标要位于源文件窗格，撤销更改
do 撤销更改
splitright选项

:tabedit two.c 新建一个窗口
gt 跳转窗口
:tab help gt 在新窗口打开帮助文档，:tab可以放在任何打开窗口的命令前
:tabonly 关闭所有其他窗口
```