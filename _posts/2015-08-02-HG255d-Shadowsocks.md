---
layout: post
title: HUAWEI HG255D 刷一个能用的Shadowsocks包。解决各种依赖。
categories: [linux, openwrt]
tags: [shadowsocks, linux]
---
<h2>{{ page.title }}</h2>

##前言

    科学上网一直是困扰着技术人员的一个问题，采购一个梯子的确是一个
    不错的选择，但对于我这种把信息泄漏作为作为第一考虑因素的人来说
    一切梯子都是渣，遇到ZF就妥协。
    今天给大家带来的是，利用4年前的路由设备结合ss来科学上网，网上
    其实有很多教程，包括刷机呀，制作包呀。但都是较新的固件，很多
    版本已经停止维护，很多包依赖已经无法解决，这里给大家带来的是
    在HG255D上的教程。

###先刷入大概是2012年的pandorabox， 

>pandorabox是openwrt 的定制固件，集成常用的软件，非常方便。不知道
现在还有没有人维护。

刷机教程我就跳过了，网上一大把，直接拿上包，

[下载链接](http://pan.baidu.com/s/1i3pNtbv) 密码: 5hr4

里面有三个文件,其中bin文件就是我们要使用的，其它两个ipk是拨号使用的
conf文件实测，已经不能update,放弃。

    opkg.conf
    PandoraBox-ralink-hg255d-r353-20140505.bin
    libpcap_1.1.1-2_ramips.ipk
    luci-app-x3c8021x_0.11+svn9957-1_ramips.ipk

刷机完成

![](https://41.media.tumblr.com/7497999570a32deda53c7e6be1d0a238/tumblr_nsfg4b814z1r68ev5o1_540.png)

然后ssh连到192.168.1.1 依次install 两个ipk,文件。


###最后一步安装下面的ss包(已经把外层剥离，只有data.tar)

明白人应该都知道，ss的支持版本里并无我们这个cpu型号，所以直接用ipk 安装并不能成功。
所以需要这个data.tar.原理很简单，请自行google相应的 deb,ipk,包打包规则。

[data.tar下载](http://pan.baidu.com/s/1o69VLI6)

`scp data.tar root@192.168.1.1:/tmp`
`tar -xvf data.tar -C /`

####测试ss-local 是否成功

![](https://40.media.tumblr.com/ca4bbb37bc7a24f8eec4763982e89591/tumblr_nsfgekgywr1r68ev5o1_540.png)

至此全部完成 你就不会报如下错误啦。

`can't resolve symbol 'epoll_create1' in lib 'ss-local`
