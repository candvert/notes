## 中文版
```
i 插入
a 附加
A 在行尾附加
x 删除单个字符
d$ 删除光标到行尾的内容
w 移动到下一个单词或标点符号
Shift + w 移动到下一个单词，忽略标点符号
b 移动到上一个单词或标点符号
Shift + b 移动到上一个单词，忽略标点符号
CTRL-F 向下滚动屏幕
CTRL-B 向上滚动屏幕
e move to the end of a end
0 移动到行首
$ 移动到行尾
d3w
dd 删除一行
u 撤销
U 撤销一行的修改
Ctrl + R 重做redo
p 将之前删除或复制的文本复制到光标后
P 将之前删除或复制的文本复制到光标前
rx 将光标处的字符替换为x
rw 将光标处的字符替换为w
ce 删除光标到单词末尾的内容并进入编辑模式
cw
c$
Ctrl + G 显示文件status
20G 移动到第20行
G 移动到文件末尾
gg 移动到文件开头

/ok 搜索ok
?ok 反向搜索ok
/ok\c 搜索ok忽略大小写
n 下一个
N 上一个
Ctrl + o 返回原来的位置
Ctrl + i 和Ctrl + o相反

% 移动光标到匹配的(), {}, []
:s/old/new 将第一个匹配的old替换为new
:s/old/new/g 将该行匹配的old替换为new
:5,8s/old/new/g 将第5行（含）到第8行（含）匹配的old替换为new
:%s/old/new/g 将文件中所有old替换为new
:%s/old/new/gc 找到所有匹配的old，并挨个询问是否替换为new
:!ls 执行shell命令ls
:w ok 将文件内容保存到文件ok

v 进入Visual selection，可以移动光标选择文本
d 删除选中的文本
:w ok 将选中的文本保存到文件ok

:r ok 将文件ok中的内容复制到光标的下一行
:r !ls 将ls命令的输出复制到光标的下一行

R 进入替换模式，输入的字符替换原有字符
y 复制文本
yw 复制单个单词
yy 复制一行

:set ic 设置忽略大小写
:set noic 取消忽略大小写
:set invic 取消忽略大小写（在设置前加上inv，反转该设置）
:set hls 设置搜索高亮
:set nohlsearch 取消搜索高亮

:help 帮助页面
:help set 显示set命令的说明
Ctrl + w两次 跳转窗口


Tab 补全命令
Ctrl + d 补全命令
Ctrl + n 下一个命令
Ctrl + p 上一个命令

"ayiw 将光标所在单词复制到寄存器a
"bdiw 将光标所在单词删除，并复制到寄存器b
<Ctrl-r>a<ESC> 进入编辑模式后，将寄存器a的内容复制到光标所在位置
:reg 查看寄存器

<Ctrl-r>=60*60*24 <ENTER> 进入编辑模式后，将60*60*24的结果填到光标所在位置
<Ctrl-r>=system('ls -l')




CTRL-] jump to a tag under the cursor
CTRL-O jump back
x 删除单个字符
diw 删除单个单词
daw 删除单词及周围的空格
dw 删除光标位置（包含）到下一个单词首字母前的内容
d0 删除光标位置（不包含）到行首的内容
d^ 删除光标位置（包含）到该行首个非空白字符间的内容
dG 删除该行（包含）到文件末尾的所有内容
d20G 删除该行（包含）到第20行（包含）的所有内容
db 删除光标之前的所有字符直到当前单词的开头
d$ 删除光标到行尾的内容
dd 删除一行
J 将两行合并，即删除两行间所有空白字符并保留一个空格
o 在光标的下面新增一行并进入插入模式
O 在光标的上面新增一行并进入插入模式
fa 跳转到该行的下一个字符a所在位置
ZZ writes file and exits
w move the cursor forward a word
b move to the start of the previous word
e move to the end of a end
ge move to the previous end of a word
^ move to the first non-blank character of the line
0 move to the beginning of a line
$ move to the end of a line
fx search forward in the line for the single character x
Fx search backward
% move to the matching parenthesis
CTRL-U scroll up half a screen of text
CTRL-D scroll down half a screen of text
zz put the cursor line at the middle
zt put the cursor line at the top
zb put the cursor line at the bottom


许多改变文本的命令是由一个operator和一个motion组成的
比如删除命令d
d是一个操作符
列举3个motion：
	w光标位置到单词末尾的内容
	e光标位置到单词末尾的内容
	$光标到行尾的内容
```
## 英文版
```
CTRL-] jump to a tag under the cursor
CTRL-O jump back
x delete a character under the cursor
diw 删除一个单词
daw 删除单词及周围的空格
dw 删除光标位置到单词末尾的内容
db 删除光标之前的所有字符直到当前单词的开头
dd delete a line
J join two lines together
u undo
CTRL-R redo
o open a line below the cursor and enter insert mode
O open a line above the cursor and enter insert mode
ZZ writes file and exits
w move the cursor forward a word
b move to the start of the previous word
e move to the end of a end
ge move to the previous end of a word
^ move to the first non-blank character of the line
0 move to the beginning of a line
$ move to the end of a line
fx search forward in the line for the single character x
Fx search backward
% move to the matching parenthesis
CTRL-U scroll up half a screen of text
CTRL-D scroll down half a screen of text
CTRL-E scoll down a line
CTRL-Y scoll up a line
CTRL-F scroll down a whole screen
CTRL-B scroll up a whole screen
zz put the cursor line at the middle
zt put the cursor line at the top
zb put the cursor line at the bottom



Many commands that change text are made from an [operator](operator) and a [motion](navigation).
The format for a delete command with the [d](d) delete operator is as follows:

    d   motion

  Where:
    d      - is the delete operator.
    motion - is what the operator will operate on (listed below).

  A short list of motions:
    [w](w) - until the start of the next word, EXCLUDING its first character.
    [e](e) - to the end of the current word, INCLUDING the last character.
    [$]($) - to the end of the line, INCLUDING the last character.

  Thus typing `de`{normal} will delete from the cursor to the end of the word.

NOTE: Pressing just the motion while in Normal mode without an operator
      will move the cursor as specified.
```