---
layout: page
title: Index
tagline: thoughts, notes, tutorials, howto’s etc…
---

<style> 
.div-a{ float:left;width:49%}
.div-b{ float:left;width:49%}
.div-c{ float:left;width:100%}
</style> 


<div class="div-a">
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
</div>

<div class="div-b">
<object type="application/x-shockwave-flash" style="outline:none;" data="http://cdn.abowman.com/widgets/hamster/hamster.swf?" width="300" height="225"><param name="movie" value="http://cdn.abowman.com/widgets/hamster/hamster.swf?"></param><param name="AllowScriptAccess" value="always"></param><param name="wmode" value="opaque"></param></object>
</div>


<div class="div-c">

    I am a full time working for one of the general firm, The purpose was to create some sort of a Knowledge base/ Cheat Sheets to contain various information about Ops, Security, Operating systems and more for my day to day use, since then the blog has changed and i guess it will continue to in the future.

    Hope you’ll find this place useful….

    Enjoy your staying.
    mail me: "enplcGFpZ2hAZ21haWwuY29tCg=="

</div>


[RSS](http://doomzhou.github.io/rss.xml)
