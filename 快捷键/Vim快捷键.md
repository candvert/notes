## 中文版
```
i 插入
a 附加
A 在行尾附加
x 删除单个字符
d$ 删除光标到行尾的内容
w move the cursor forward a word
e move to the end of a end
0 移动到行首
$ 移动到行尾
d3w
dd 删除一行
u 撤销
U 撤销一行的修改
Ctrl + R 重做redo
p 将之前删除的文本复制到光标后
P 将之前删除的文本复制到光标前
rx 将光标处的字符替换为x
rw 将光标处的字符替换为w
ce 删除光标到单词末尾的内容并进入编辑模式
cw
c$
Ctrl + G 显示文件status
20G 移动到第20行
G 移动到文件末尾
gg 移动到文件开头



CTRL-] jump to a tag under the cursor
CTRL-O jump back
x 删除单个字符
diw 删除单个单词
daw 删除单词及周围的空格
dw 删除光标位置到下一个单词首字母前的内容
db 删除光标之前的所有字符直到当前单词的开头
d$ 删除光标到行尾的内容
dd 删除一行
J 将两行合并，即删除两行间的回车符
u 撤销
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