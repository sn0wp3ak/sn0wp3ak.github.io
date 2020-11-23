---
layout: page
title: Tag
---

<h2>Tags</h2>

<ul>
    {% for post in site.posts %}
    	{% if post.tags[0] %}
    		<li><a>{{ post.tags }}</a></li>
	{% endif %}
    {% endfor %}
</ul>





