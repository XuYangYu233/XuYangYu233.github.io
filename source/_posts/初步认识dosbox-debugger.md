---
title: 初步认识dosbox_debugger
date: 2020-10-09 16:43:21
tags: masma
cover: /imgs/miku1.jpg
coverWidth: 1600
coverHeight: 900
---

搬一篇原来写的东西试试效果

<!--more-->

## 0.这是什么东西？
DOSBox debugger是一款由DOSBox原作者为DOSBox量身打造的调试器，几乎所有运行于DOSBox上的程序都可以用它进行调试。0.74-3版本的下载地址为http://source.dosbox.com/dosbox-74-3-debug.exe
![dosbox_debugger的界面](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xOTc4ODY4My1mNTUzYTZhODhmMjlhYWIzLnBuZw?x-oss-process=image/format,png)
## 1.为什么选择DOSBox debugger?
不同的人可能有不同的理由，我的理由是为了学习16位和32位masm汇编。在64位操作系统上学习汇编有很多方式，那么为什么是dosbox及其debugger？
#### （1）开门见山地说吧，最重要的一点，就是在masm-code插件的加持下，dosbox debugger可以完美地兼容于vscode
vscode，永远滴神
#### （2）VS上同样可以写masm，而且VS自带的调试器功能更强大，界面更友好，那为啥不用VS？一是因为VS实在太大了，二是因为vscode永远滴神
三是因为VS配置起来比较麻烦，而我比较懒
#### （3）emu8086？毕竟是8086，不支持32位
#### （4）masm for windows？本体就是个文本编辑器，主要原因是丑
众所周知，好看的工作环境有利于提高工作效率
#### （5）Red asm/Visual asm/等等？别问，问就是vscode天下第一
#### （6）为什么不用dosbox原本的debug.exe?功能少而且难以查看32位寄存器
## 2.安装
dosbox怎么装我就不说了，网上一大堆。从上面那个链接下载了debugger之后，将其放在dosbox.exe同一个文件夹下，就基本ok了，双击该exe就可以打开了。

如果是使用了masm-code的vscode，那么就放在C:\Users\你的名字\AppData\Roaming\Code\User\globalStorage\kaixa.masm-code这个文件夹里，并把这个debugger的文件名改成DOSBox.exe，把原来的DOSBox.exe改成另外的名字。这样就可以让你在使用vscode时通过ctrl+shift+p或者F1直接启动dosbox及其debugger![在这里启动](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy8xOTc4ODY4My03ODA4ZjZiNDRiZDFkMDNmLnBuZw?x-oss-process=image/format,png)
弹出dosbox窗口后，输入debug xxxx.exe并按下回车，dosbox debugger就会开始工作了。
## 3.使用
这款调试器有别于现在的大多数高级语言调试器，鼠标基本是完全没用的，想要操作它必须得用键盘上的各种键或者在其里面敲命令。下面就讲讲几个常用的键位：
单步运行 F10
查看数据段内存 alt+D（记得等程序将数据段地址传给DS之后）
然后没了。由于本人能力所限，目前常用的就这两条。当然，还是把其他的键或指令都展示一下：
>F3 / F6-历史记录中的上一个命令。
F4 / F7-历史中的下一个命令。
F5-运行。
F9-设置/删除断点。
F10 / F11-跨步/追溯到指令。
ALT + D / E / S / X / B-将数据视图设置为DS：SI / ES：DI / SS：SP / DS：DX / ES：BX。
esc-清除输入行。
上/下-移动代码视图光标。
Page Up / Down-滚动数据视图。
Home / End-滚动日志消息。
BP [段]：[偏移]-设置断点。
BPINT [intNr] *-设置中断断点。
BPINT [intNr] [ah] *-用ah设置中断断点。
BPINT [intNr] [ah] [al]-使用ah和al设置中断断点。
BPM [段]：[偏移量]-设置内存断点（内存更改）。
BPPM [选择器]：[偏移量]-设置pmode内存断点（内存更改）。
BPLM [线性地址]-设置线性内存断点（内存更改）。
BPLIST-列出断点。
BPDEL [bpNr] / *-删除断点nr /全部。
C / D [段]：[偏移量]-设置代码/数据视图地址。
DOS MCBS-显示内存控制块链。
INT [nr] / INTT [nr]-执行/跟踪到中断。
LOG [num]-写入cpu日志文件。
LOGS / LOGL [num]-写入长/短的cpu日志文件。
HEAVYLOG-在dosbox退出时启用/禁用自动cpu日志。
ZEROPROTECT-启用/禁用零代码执行检测。
SR [reg] [value]-设置寄存器值
SM [seg]：[off] [val] [。] ..-用以下值设置存储器。
IV [seg]：[off] [name]-为内存地址创建var名称。
SV [filename]-将var列表保存在文件中。
LV [filename]-从文件加载var列表。
ADDLOG [消息]-将消息添加到日志文件。
MEMDUMP [seg]：[off] [len]-将内存写入文件memdump.txt。
MEMDUMPBIN ：[o] [len]-将内存写入文件memdump.bin。
SELINFO [segName]-显示选择器信息。
INTVEC [文件名]-将中断向量表写入文件。
INTHAND [intNum]-将代码视图设置为中断处理程序。
CPU-显示CPU状态信息。
GDT-列出GDT的描述符。
LDT-列出LDT的描述符。
IDT-列出IDT的描述符。
PAGING [页面]-显示页面表的内容。
EXTEND-切换其他信息。
TIMERIRQ-运行系统计时器。
HELP-帮助

来源：https://www.vogons.org/viewtopic.php?t=3944
## 4.结语
其实也没啥好说的，这个debugger还是比较傻瓜的。
当然，感谢dosbox及其debugger的作者，感谢Wgagaxnunigo sf让我知道了这款强大的调试器，感谢masm-code的作者kaixa，感谢我在知乎上认识的wz大佬（有空可以去看看她的关于vscode配置masm环境的文章[https://zhuanlan.zhihu.com/p/105268949](https://zhuanlan.zhihu.com/p/105268949)）
由于本人水平所限，此片文章中可能存在一些错误或繁琐之处，还望各路大佬多多指教。
