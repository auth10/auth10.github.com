---
layout: page
title: Auth10 Blog
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.content | preview }}
    <a href="{{ post.url }}">Read More</a>
  {% endfor %}
</ul>
