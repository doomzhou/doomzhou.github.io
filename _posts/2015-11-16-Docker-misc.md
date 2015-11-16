---
layout: post
title: Docker,centos7拾遗
categories: [Gnu/linux]
tags: [Centos7, Docker]
---

Intro
===
>一些centos7下使用docker的记录

###禁用firewalld 2 iptables

{% highlight bash linenos %}
systemctl stop firewalld
systemctl mask firewalld
yum install iptables-services -y
systemctl enable iptables
{% endhighlight bash linenos %}

####若使用minimize版本的安装，可能会出现提示

    iptables: Saving firewall rules to /etc/sysconfig/iptables: /etc/init.d/iptables: line 274: restorecon: command not found

这是因为selinux没有安装的缘故，缺少一个组件。安装policycoreutils即可。

`yum install policycoreutils -y`

