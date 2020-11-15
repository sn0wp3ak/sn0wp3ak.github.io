---
layout: page
title: Categories
---

<h2>Categories</h2>

<ul>
    {% for post in site.posts %}
    {% if post.categories %}
    <li><a>{{ post.categories }}</a></li>
    {% endif %}
    {% endfor %}
</ul>









