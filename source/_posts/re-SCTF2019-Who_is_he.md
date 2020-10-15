---
title: 're_[SCTF2019]Who is he'
date: 2020-10-15 22:11:31
tags: re
cover: /imgs/huan.jpg
coverWidth: 1200
coverHeight: 650
---
一道解题工具非常有特色的逆向题
<!--more-->

## 0

Unity游戏。按照惯例直接将Assembly-CSharp.dll放入dnSpy查看，马上找到关键函数和信息：

![07YzX8.png](https://s1.ax1x.com/2020/10/15/07YzX8.png)

找到key的引用处，可以发现是DES加密，IV和key相同：

![07tpnS.png](https://s1.ax1x.com/2020/10/15/07tpnS.png)

将信息放入CyberChef进行解密：

![07N9v6.png](https://s1.ax1x.com/2020/10/15/07N9v6.png)

输入程序发现不正确？？？

## 1

看了wp才知道这个地方的信息原来是假的，要看真信息还得用CE修改器，于是赶紧去三大妈上找了个汉化版：

![07UdfA.png](https://s1.ax1x.com/2020/10/15/07UdfA.png)

运行Who is he.exe，在CE里添加该进程，先搜索一下字符串，注意要把UTF-16那个框打勾，发现出现了"Emmmm"的地方有两处：

![07U4lq.png](https://s1.ax1x.com/2020/10/15/07U4lq.png)

点击查看内存，然后点击搜索内存，输入"Emmmm"，照例勾上UTF-16，然后可以搜出第一处：

![07U0SI.png](https://s1.ax1x.com/2020/10/15/07U0SI.png)

下面有一串base64编码，将其复制到CyberChef里面，照刚才的方法进行DES解密，发现这一处也不是真的

![07UIXV.png](https://s1.ax1x.com/2020/10/15/07UIXV.png)

继续搜可以搜出第二处：

![07U70U.png](https://s1.ax1x.com/2020/10/15/07U70U.png)

需要注意的是这一处的DES解密的key由1234改成了test：

![07dux1.png](https://s1.ax1x.com/2020/10/15/07dux1.png)

将此处的密文和key拿到CyberChef里解密，解出了一串比较正常的字符串：

![07U560.png](https://s1.ax1x.com/2020/10/15/07U560.png)

将这串输入程序，发现成功了，flag为flag{She_P1ay_Black_Hole_Very_Wel1!LOL!XD!}

## 2

以前玩MHW的时候就听说过CE的大名，但为了不降低游戏的乐趣就一直没用过，想不到第一次使用居然是在这种地方。。。
