---
layout: post
title: 用Newsbeuter替换Feedly做Rss阅读(双12鲜果Rss死了)
categories: [Gnu/linux]
tags: [linux, Rss]
---

###需要起因

>Rss能够节约程序员，运维狗的生命。而从GR死后，我心已经不能平静  
>现在鲜果也死了，我的小心脏(虽然我不用).  
>另一方面嘛，terminal下装B是我等linuxer的良方。  
>来吧让我一起把节约生命和装B结合到不起。

###操作步骤

>前言:无论你是GR(一脸泪),还是Feedly,鲜果,你都得知道`OPML`。

####OPML长什么样

![OPML](https://38.media.tumblr.com/431bbe262409ceecef8d9a3cde58dde4/tumblr_ngi2yuA8nJ1r68ev5o1_1280.png)

>对了它是XML。

####OPML从哪里来

![OPML](https://38.media.tumblr.com/e4a615d6bfd8dabf9f1299638b8b9497/tumblr_ngi3d5i3RF1r68ev5o1_1280.png)

>对肯定有export,话说Feedly竟然没有按钮，要自己尝试这个Url.

####主角Newsbeuter

>官网是 [这里](http://newsbeuter.org/)  
>安装使用各发行版包管理。google之

####Newsbeuter使用上面OPML的导出文件

`newsbeuter -i feedly.opml`

####最后长这样就能看了。

![img](https://33.media.tumblr.com/7524aba83dc6c298c818d69ac65629a8/tumblr_ngi3qiQdxs1r68ev5o1_1280.png)

###延伸
*  OPML是Outline Processor Markup Language的缩写。OPML也是一种XML规范的文件格式。在RSS中，它的主要用途是用来交换RSS Feed。一个OPML文件中可以包含大量的RSS Feed信息，如果一个RSS阅读器支持OPML文件的话，导入 RSS Feed就比较容易了。
