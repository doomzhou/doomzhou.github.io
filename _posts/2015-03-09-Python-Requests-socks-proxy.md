---
layout: post
title: Python中Request 使用socks5代理的两种方法(个人推荐方法二)
categories: [Coder]
tags: [Pythonic]
---

Intro
====

>在数据抓取的时候经常要使用到HTTP for Humans 的Requests,由于经常做些要代理的
事情，顺便查找下使用Socks5的方法。


###Example 1

`pip install requesocks`

 {% highlight python linenos %}
if is_open('https://github.com/shazow/urllib3/pull/68'):  # ;)
    import requesocks as requests
else:
    import requests

session = requests.session()
session.proxies = {'http': 'socks5://127.0.0.1:9050',
                   'https': 'socks5://127.0.0.1:9050'}
resp = session.get('https://api.github.com', auth=('user', 'pass'))
print(resp.status_code)
print(resp.headers['content-type'])
print(resp.text)
 {% endhighlight python linenos %}

>Python (Python-3.X) library for connection via SOCKS5-proxy


###Example 2

[PySocks](https://github.com/Anorov/PySocks)

`pip install PySocks`

{% highlight python %}
import socket
import socks
import requests

socks.set_default_proxy(socks.SOCKS5, "127.0.0.1", 9050)
socket.socket = socks.socksocket
print(requests.get('http://ifconfig.me/ip').text)


{% endhighlight python %}
你就会得到Tor的代理IP了。


###reference
[stackoverflow](http://stackoverflow.com/questions/12601316/how-to-make-python-requests-work-via-socks-proxy)
[PySocks](https://github.com/Anorov/PySocks)
