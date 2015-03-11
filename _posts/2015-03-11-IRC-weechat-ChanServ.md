---
layout: post
title: 使用IRCi(weechat)入门注册建立私人Chan
categories: [Linux]
tags: [IRC, weechat]
---

Intro
====

>IRC不谈了真是很好用，我把XXXXXXXXXX了，来看步骤。任何有违IRC基本准则的请PMme

###step1
    
####

    了解什么是IRC，什么是weechat
    IRC:Internet Relay Chat
    weechat:IRC的客户端，其它的还有网页版/pidgin/xchat.....
    
####安装weechat

    基本上平台都在源里面有
    一大波命令: 

`yum apt-get pacman ..... weechat `

####挑选服务器，freenode 无误了 其它的自寻

####注册Nick

`/msg NickServ REGISTER doomzhou password gmail@gmail.com`

####然后去Gmail收验证码意外是spam 里

`/msg NickServ VERIFY REGISTER doomzhou 验证码`

####查看Chan是否存在

`/msg ChanServ INFO #foo`

>>我去这个大个公司竟然没人用，

####创建Chan

`/join #foo`

####汎注册到自己下面

`/msg ChanServ REGISTER #foo 1234`

####加入机器人管理看着就专业

`/msg ChanServ SET #foo GUARD ON`


###reference
此文档无 可以来 IRC找我

