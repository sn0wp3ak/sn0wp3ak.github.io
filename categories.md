---
layout: page
title: Category
---
<div>
<h2>分类</h2>

<ul>
    {% for category in site.categories %}
    	<li>
	<a href="#{{ category[0] }}">
	<span>{{ category[0] }}</span>
	<span> -- {{ category[1] | size }}篇</span>
	</a>
	</li>
    {% endfor %}
</ul>
<div>
<br>
<hr />
<br>
<div>
{% for category in site.categories %}
	<h2 id="{{ category[0] }}">
	{{ category[0] }}
	</h2>
	<ul>
		{% for post in category[1] %}
			<li>
			<a href="{{ post.url | prepend: site.baseurl | replace: '//', '/'}}">
        			{{ post.title }} 
        		</a>
			</li>
		{% endfor %}
	</ul>
{% endfor %}
<div>






