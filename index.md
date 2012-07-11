---
layout: page
title: Auth10 Blog
---
{% include JB/setup %}


<div class="posts">
  {% for post in site.posts %}
  	<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    {{ post.content | split:"<!-- end preview -->" | first }}
    <a href="{{ post.url }}" class="readmore">Read More...</a>
    <hr/>
  {% endfor %}
</div>
