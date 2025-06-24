---
layout: default
title: Bits & Bytes da IA
permalink: /index/
---

<ul class="posts_latest">
    {% for post in site.posts limit: 2 %}
    <li>
        <img src="{{ site.baseurl}}/{{ post.image }}">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
        <p> {{post.author}} </p>
        <div>
            <p>{{ post.date | date: "%d %b %Y" }}</p>
            <p></p>
        </div>
    </li>
    {% endfor %}
</ul>

<h2>Recentes</h2>
<ul>
    {% for post in site.posts %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>
