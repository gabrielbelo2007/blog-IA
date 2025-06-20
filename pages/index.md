---
layout: default
title: Bits & Bytes da IA
permalink: /index/
---

<h2 id="posts">Recentes</h2>
<ul>
    {% for post in site.posts limit: 2 %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>

<h2 id="posts">Posts</h2>
<ul>
    {% for post in site.posts %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
