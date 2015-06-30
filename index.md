---
layout: page
title: Welcome
tagline: Supporting tagline
---
{% include JB/setup %}

Welcome to visit the site of Jack Jack Attack! As a <a href="http://devbootcamp.com">Dev BootCamp</a> graduate, my blog here includes some discussion about certain web development topics, such as Javascript, Ruby and Rails. Thanks for coming and looking forward to have your tweets on <a href="http://twitter.com/?status=@xjlin0">@xjlin0</a> or your comments on disqus.

### Some of my thoughts:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>