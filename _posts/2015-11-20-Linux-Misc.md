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



###notify-send && dunst 实现通知功能,比如翻译

####安装　  

`pacman -S dunst`

####启动   

`dunst `  

####notify-send   

`pacman -S libnotify`
`notify-send 'test' 'test'`


见引用
[ref](http://linuxwireless.org/en/users/Drivers/iwlwifi/)

下载见
[提取码：cfc4](http://yunpan.cn/cL9XkZ458pcXT)


####Linux 使用key验证后无密码登录维护怎么处理Give root password for maintenance

* select boot item
* 'e' kernel line
* add 'init=/bin/bash'
* 'b' to boot kernel
* mount -o remont,rw /
* passwd root
* reboot

[引用](https://www.md3v.com/linux-give-root-password-for-maintenance-lost-password)

####echo 修改密码

1. `echo -e "new_password\nnew_password" | (passwd --stdin $USER)`

2. `echo "password:name" | chpasswd`


