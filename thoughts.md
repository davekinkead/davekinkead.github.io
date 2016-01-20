---
layout: page
title:  Recent Confusions
permalink: /thoughts/
---


<ul class="posts">
  {% for post in site.posts %}
    <li class="post">
      <div class="title"><h3><a href="{{ post.url }}">{{ post.title }}</a></h3><span class="meta">{{ post.date | date_to_string }}</span></div>
      <div class="excerpt">{{ post.excerpt }}</div>
    </li>
  {% endfor %}
</ul>