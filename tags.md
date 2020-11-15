---
layout: page
title: Tags
---

<h2>Tags</h2>

<ul>
    {% for post in site.posts %}
    	{% if post.tags != "" %}
    		<li><a>{{ post.tags }}</a></li>
		{% endif %}
    {% endfor %}
</ul>





