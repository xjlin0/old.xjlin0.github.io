---
layout: page
title: Welcome
tagline: Supporting tagline
---
{% include JB/setup %}

Here is the site for Jack Jack Attack! I am a Dev BootCamp graduate. My ongoing website here will update some discussion about my web development path, can be Javascript, Ruby and of course Rails. Looking forward to have your tweet on <a href="http://twitter.com/?status=@xjlin0">@xjlin0</a> or your comments on disqus.

### Some of my thoughts:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>