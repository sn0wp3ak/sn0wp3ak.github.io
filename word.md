---
layout: page
title: Word
---
<h2>词汇积累</h2>
<hr>
<div>
{% for category in site.categories %}
	{% if category[0] == "Word" %}
	<ul>
		{% for post in category[1] %}
			<li class="word">
			<a href="{{ post.url | prepend: site.baseurl | replace: '//', '/'}}">
				{{ post.title }}
			</a>
			</li>
		{% endfor %}
	</ul>
	{% endif %}
{% endfor %}
</div>
