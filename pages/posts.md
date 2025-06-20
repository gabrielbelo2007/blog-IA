---
layout: default
title: Posts
permalink: /posts
---

<ul>
    {% for post in site.posts limit: 5 %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>