---
layout: post
title: Archlinux下Java程序的丑陋字体调整
categories: [linux]
tags: [JAVA, Fonts]
---
<h2>{{ page.title }}</h2>


我的FVWM的字体明明很好看，可是JAVA程序真的很难看呀，找下原因


### 难看的字体

![](https://66.media.tumblr.com/a5add06d1d13421c6b27a8208502faeb/tumblr_o7eyzkubik1r68ev5o4_1280.png)
![](https://66.media.tumblr.com/725570582f2644d1f1903cfdcf06111b/tumblr_o7eyzkubik1r68ev5o2_540.png)

### 调整后的字体

![](https://66.media.tumblr.com/71a6e0d25cf3c1aba188156bbef6dd8b/tumblr_o7eyzkubik1r68ev5o1_1280.png)
![](https://66.media.tumblr.com/3183ce2885cb59d924a51bc90301e286/tumblr_o7eyzkubik1r68ev5o3_1280.png)


### 解决方案


加个参数就是这么简单

    -Dawt.useSystemAAFontSettings=on
