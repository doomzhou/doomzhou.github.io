---
layout: post
title: Python中list的元素出现频率
categories: [Coder]
tags: [Pythonic]
---

Intro
====

>两种方法统计list中的元素出现次数。


###Example 1

 {% highlight python linenos %}
 >>> a = [1,1,1,1,2,2,2,2,3,3,4,5,5]
 >>> d = {x:a.count(x) for x in a}
 >>> d
 {1: 4, 2: 4, 3: 2, 4: 1, 5: 2}
 >>> a, b = d.keys(), d.values()
 >>> a
 dict_keys([1, 2, 3, 4, 5])
 >>> b
 dict_values([4, 4, 2, 1, 2])
 {% endhighlight python linenos %}


###Example 2(In Python 2.7+) 

{% highlight python %}
>>> import collections
>>> a = [1,1,1,1,2,2,2,2,3,3,4,5,5]
>>> counter=collections.Counter(a)
>>> d = collections.Counter(a)
>>> d
Counter({1: 4, 2: 4, 3: 2, 5: 2, 4: 1})
>>> a, b = d.keys(), d.values()
>>> a
dict_keys([1, 2, 3, 4, 5])
>>> b
dict_values([4, 4, 2, 1, 2])
{% endhighlight python %}


###reference
[stackoverflow](http://stackoverflow.com/questions/2161752/how-to-count-the-frequency-of-the-elements-in-a-list)
