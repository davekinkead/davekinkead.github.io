---
layout: page
title:  Talks & Presentations
permalink: /talks/
---

<ul class="posts">
  {% for post in site.posts %}
  {% if post.category == 'talks' %}
    {% include post_summary.html %}
    {% endif %}
  {% endfor %}
</ul>