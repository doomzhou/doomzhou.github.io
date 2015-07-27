---
layout: post
title: 在Archlinux在简易安装配置Syslog-ng
categories: [Gnu/linux]
tags: [linux, log]
---

##需求起因

    多数网络设备，开源服务都会需要日志服务器，连hist都可以保存
    syslog进行审计,所以简易的在Archlinux在安装syslog，以供调试



##操作步骤

###安装syslog-ng

`pacman -S syslog-ng`

###配置syslog-ng

 {% highlight python %}
  mkdir /var/log/remote
 cat << "EOF" >> /etc/syslog-ng/syslog-ng.conf
 source net { udp(); };
 destination remote { file("/var/log/remote/remote.log"); };
 log { source(net); destination(remote); };
 EOF
 {% endhighlight python %}

###启动syslog-ng

`systemctl restart syslog-ng`


###测试成功

`logger -n 192.168.1.123 'remote syslog test'`


##引用
[link1](https://wiki.archlinux.org/index.php/Syslog-ng#Configuring_as_a_loghost)
