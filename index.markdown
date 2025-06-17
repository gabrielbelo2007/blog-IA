---
layout: default
title: Bem-vindo ao teste de Blog!
---

# Bem-vindo!

Aqui sao compartilhados posts sobre IA.

[Ver ultimos posts](#posts)

<h2 id="posts">Ultimos posts</h2>
<ul>
    {% for post in site.posts limit: 5 %}
    <li>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
    {% endfor %}
</ul>