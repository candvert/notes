- [标题](#标题)
- [标签](#标签)
- [文件内链接](#文件内链接)
- [网站链接](#网站链接)
- [图片链接](#图片链接)
- [视频链接](#视频链接)
- [音频链接](#音频链接)
- [代码块](#代码块)
- [各种效果](#各种效果)
- [分隔线](#分隔线)
- [复选框](#复选框)
- [标注](#标注)
- [内部链接](#内部链接)
- [插入文件](#插入文件)
- [嵌入网页](#嵌入网页)
## 标题
# example
## example
***
## 标签
%%标签不区分大小写，#animal和#Animal为相同标签。并且标签不能包含空格%%
#animal
#收件箱/待阅读
***
## 文件内链接
[Chapter 1](#jump)
### jump

[有 空格](#有%20空格)
[有 空格](<#有 空格>)
### 有 空格
***
## 网站链接
[baidu](https://www.baidu.com)
[baidu](https://baidu.com "main site")
<https://www.baidu.com>
***
## 图片链接
![[TheImage.png]]
![](TheImage.png)
%%通过|400x400改变图片尺寸%%
![|400x400](TheImage.png)
%%只指定|400则根据原始长宽比进行缩放%%
![|400](TheImage.png)
![](TheImage.gif)
![image|200x200](https://img2.baidu.com/it/u=396436058,219850851&fm=253&app=138&size=w931&n=0&f=JPEG&fmt=auto?sec=1708880400&t=65ff3edcf468afcb21053913c33f6cd5)
***
## 视频链接
![[2-30fps.mp4]]
## 音频链接
![[song.mp3]]
## 代码块
`#include <stdlib>`中的文本将被格式化为代码。

```cpp
#include <iostream>
```
***
## 各种效果
+ a
  - b
    * c

> kk
> 
> > dd

~~delete~~

%%comment%%

==hood==

*kk*
_kk_

**bold**
__bold__

**the _beautiful_ example**

***mixed***
___mixed___

\*\*这里的加粗不会真正的加粗\*\*
***
## 分隔线
***

---

___
## 复选框
- [x] done
- [ ] to do
***
## 标注
> [!note] 示例
这是一个标注块。
它支持 Markdown、内部链接 和 嵌入！

> [!abstract]
还有[!info]、[!todo]、[!tip]、[!success]、[!check]、[!done]、[!question]、[!help]、[!warning]、[!failure]、[!danger]、[!error]、[!bug]、[!example]、[!quote]等。

%%在[!faq]后加-默认折叠，加+默认展开%%
> [!faq]- 标注可以折叠吗？
> 可以！在可折叠标注中，当标注收起时，内容会被隐藏。

> [!question] 标注可以嵌套吗？
> > [!todo] 可以。
> > > [!example]  你甚至可以使用多层嵌套。
***
## 内部链接
跳转到另一条笔记[[newnote]]
***
## 插入文件
![[json格式]]
***
![[json格式#^示例]]
***
## 嵌入网页
<iframe src="https://bilibili.com" width="600" height="400"></iframe>


![](https://www.youtube.com/watch?v=NnTvZWp5Q7o)


![](https://twitter.com/obsdmd/status/1580548874246443010)
