---
layout: page
title: Category
---

<h2>Categories</h2>

<ul>
    {% for post in site.posts %}
    {% if post.categories[0] %}
    <li><a>{{ post.categories }}</a></li>
    {% endif %}
    {% endfor %}
</ul>









