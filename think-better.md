---
layout: category
title:  Think Better
question: What to learn how to think better?
permalink: /think-better/
---

<ul class="posts">
  {% for post in site.posts %}
  {% if post.category == 'talks' %}
    {% include post_summary.html %}
    {% endif %}
  {% endfor %}
</ul>