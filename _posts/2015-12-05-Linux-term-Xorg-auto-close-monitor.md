---
layout: post
title: Linux下关于自动关闭显示器的配置
categories: [Gnu/linux]
tags: [linux, Xorg]
---

Intro
====
>问题是这样的最近做一个展示，所以必需要保证不能关闭显示器。
这里讲自己测试的两点一是X下，一是tty下面。

###Xorg, fvwm, 等
>这一点本来不会成为问题，但是我比较爱fvwm,　无任何修饰。就是因为没
了修饰，也没了很多图形界面的工具(gnome,kde下可能就是点几下鼠标的事)

设置方法:其实就是一个命令,具体自己去玩,

`xset dpms force on` 

`xset s 0`


###tty，term下
>这个的话直接上命令

`setterm -blank 0 `


###当然少不了引用

[引用1](https://community.freescale.com/thread/323250)

[引用2](http://socol.iteye.com/blog/1039725)
