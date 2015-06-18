---
layout: page
title:  Presentations
permalink: /talks/
---

<ul class="posts">
  {% for post in site.posts %}
  {% if post.category == 'talks' %}
    <li class="post">
      <div class="title"><h3><a href="{{ post.url }}">{{ post.title }}</a></h3><span class="meta">{{ post.audience }}, {{ post.date | date_to_string }}</span></div>
      <div class="excerpt">{{ post.excerpt }}</div>
    </li>
    {% endif %}
  {% endfor %}
</ul>