---
layout: post
title: 使用IRCi(weechat)入门注册建立私人Chan
author: doomzhou
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

>我去这个大个公司竟然没人用，

####创建Chan

`/join #foo`

####注册到自己下面

`/msg ChanServ REGISTER #foo 1234`

####加入机器人管理看着就专业

`/msg ChanServ SET #foo GUARD ON`

####管理员切到私人channel
`/msg ChanServ set #foo founder doomzhou`  
`/msg ChanServ op #foo doomzhou`  
`/mode +i`  

####自邀请，防进不去

`/msg Chanserv invite #foo`


####更改为密码方式

`/msg ChanServ set #qc mlock +k passwordofyour`

###关于自建立unreal

>TODO(见qckb)

####自建SSL支持后weechat　报错解决方案　

>报错信息

{% highlight bash %}
11:23:51       me =!= | gnutls: peer's certificate is NOT trusted
11:23:51       me =!= | gnutls: peer's certificate issuer is unknown
11:23:51       me =!= | irc: TLS handshake failed
{% endhighlight bash %}

>解决方案1

[gist](https://github.com/gitterHQ/gitter/issues/459)

>解决方案2

添加cert到信任

###reference
此文档无 可以来 IRC找我

