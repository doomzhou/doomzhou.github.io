---
layout: page
title: Index
tagline: thoughts, notes, tutorials, howto’s etc…
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

    I am a full time working for one of the general firm, The purpose was to create some sort of a Knowledge base/ Cheat Sheets to contain various information about Ops, Security, Operating systems and more for my day to day use, since then the blog has changed and i guess it will continue to in the future.

    Hope you’ll find this place useful….

    Enjoy your staying.
    mail me: "enplcGFpZ2hAZ21haWwuY29tCg=="

[RSS](http://blog.gohjkl.com/rss.xml)
