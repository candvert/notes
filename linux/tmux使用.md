## 会话（session）
```sh
终端输入tmux进入新会话
终端输入tmux ls列出所有会话
终端输入tmux attach会进入上次离开的会话
终端输入tmux attach -t <number>进入指定会话
终端输入tmux kill-session -t <number>关闭指定会话
Ctrl+b d：离开当前会话
Ctrl+b :kill-session关闭会话
```
## 窗口（Window）
```sh
Ctrl+b c：创建一个新窗口
Ctrl+b <number>：切换到指定窗口
Ctrl+b p：切换到上一个窗口
Ctrl+b n：切换到下一个窗口
Ctrl+b w：从列表中选择窗口，按上下键进行选择
Ctrl+b &：关闭当前窗口
Ctrl+b ,：窗口重命名
```
## 窗格（Pane）
```sh
Ctrl+b %：划分左右两个窗格
Ctrl+b "：划分上下两个窗格
Ctrl+b q：显示窗格编号
Ctrl+b q <number>：切换到指定窗格
Ctrl+b <arrow key>：光标切换到其他窗格
Ctrl+b o：光标切换到下一个窗格
Ctrl+b ;：光标在两个窗格间来回切换
Ctrl+b x：关闭当前窗格
Ctrl+b !：将当前窗格拆分为一个独立窗口
Ctrl+b z：当前窗格全屏显示，再使用一个变回原来大小
```