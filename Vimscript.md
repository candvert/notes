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
```