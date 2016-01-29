---
layout: page
title: Posts
---

{% for post in site.posts %}
  <h2 class="post-title"><a href="{{ post.url }}">
    {{ post.title }}
  </a></h2>
  <hr/>
{% endfor %}
