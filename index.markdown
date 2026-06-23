---
layout: page
title: Articles
permalink: /
---

Welcome to Xiao's technical notes.

## All Articles

<ul>
{% for post in site.posts %}
  <li>
    <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    —
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>
