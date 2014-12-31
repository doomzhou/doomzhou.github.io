---
layout: post
title: iphone 上使用burpsuite 抓包
categories: [security]
tags: [Burpsuite,iPhone]
---
<h2>{{ page.title }}</h2>

#Intro

>为什么某些app不能使用Burpsuite进行抓包分析，或者抓包正常，但是Forward某些包后  
>没有正常返回。因为某些请求使用了https双向验证这就要用到新版的Burpsuite的证书,
>导入导出功能了。具体操作

###问题图例

    以某app为例，当中有个请求是https的

![img1](https://38.media.tumblr.com/25016e67d108c5c6fe6a7388d8416a1e/tumblr_nhe4ei1NOZ1r68ev5o1_1280.png)

###问题修复步骤

####1.获得Burp的证书,首先是配置浏览器走Burp的代理。然后请求http://burp/cert,保存证书。
![img2](https://33.media.tumblr.com/7bee8260c35566e748af00932e2a6ef0/tumblr_nhe4ei1NOZ1r68ev5o2_1280.png)

####2.使用safari打开你的证书，点击install.
![img3](https://38.media.tumblr.com/95e6b820fcdab88696ca022b57091306/tumblr_nhe43xVEAA1r68ev5o1_1280.jpg)
