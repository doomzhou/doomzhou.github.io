---
layout: post
title: mount cifs Permission denied
categories: [linux]
tags: [mount, linux]
---
<h2>{{ page.title }}</h2>

##报错信息

    mount error(13): Permission denied
    Refer to the mount.cifs(8) manual page (e.g. man mount.cifs)

##解决方案

>加参数-o sec=ntlm

    /etc/fstab
    //192.168.56.1/data                             /data    cifs   sec=ntlm,username=administrator     0 0

or

    #mount -t cifs -o username=administrator,sec=ntlm //192.168.56.1/data /data

##原因参考

>[引用](https://bbs.archlinux.org/viewtopic.php?id=159915)

    大概原因是好像是新的linux kernel(3.8+)，要手动加-o sec=ntlm
