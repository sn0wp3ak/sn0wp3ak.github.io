---
layout: page
title: Categories
---

<h3>所有分类</h3>

<ul>
    {% for post in site.posts %}
    {% if post.categories %}
    <li><a>{{ post.categories }}</a></li>
    {% endif %}
    {% endfor %}
</ul>









