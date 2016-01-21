---
layout: post
title: Kali做无线审计时碰到无线网卡名乱跳问题
categories: [linux ]
tags: [Kali, airmon-ng]
---
<h2>{{ page.title }}</h2>

##问题，在kali1系列的时候是没有问题的

`root@kali:~# airodump-ng wlan0mon`

    ioctl(SIOCSIWMODE) failed: Device or resource busy

    ARP linktype is set to 1 (Ethernet) - expected ARPHRD_IEEE80211,
    ARPHRD_IEEE80211_FULL or ARPHRD_IEEE80211_PRISM instead.  Make
    sure RFMON is enabled: run 'airmon-ng start wlan0mon <#>'
    Sysfs injection support was not found either.

每一次的dump都会在wlan0mon后面增加'mon' :(


###解决方案

`ifconfig wlan0mon down`
`iwconfig wlan0mon mode monitor`
`ifconfig wlan0mon up`

这样就能在　`airodump-ng wlan0mon` 了


####同理要提一下，使用相应的gui工具时候也一定要在添加完interface后，另开终端执行

>比如在kali fern wifi cracker下，一定要按顺序操作，保证不出错

`ifconfig wlan0mon down`
`iwconfig wlan0mon mode monitor`
`ifconfig wlan0mon up`

