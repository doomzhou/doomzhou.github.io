---
layout: post
title: Linux拾遗
categories: [Gnu/linux]
tags: [linux, centos]
---

##一大波列表

###yum transcation hangs
{% highlight bash %}
su -c 'yum clean all && rpm --rebuilddb'
su -c 'package-cleanup --problems'
su -c 'yum --setopt=tsflags=noscripts remove zarafa*'
{% endhighlight bash %}
>If that doesn't work, try this:

`su -c 'rpm -e --noscripts zarafa*'`


###intel corporation wireless 7265 linux

`# cp iwlwifi-*.ucode /lib/firmware `

见引用
[ref](http://linuxwireless.org/en/users/Drivers/iwlwifi/)

下载见
[提取码：cfc4](http://yunpan.cn/cL9XkZ458pcXT)
