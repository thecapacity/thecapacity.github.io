---
layout: default
title: thecapacity
---

# Welcome

## Information
* blog.thecapacity.org

<ul>
  {% for post in paginator.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
