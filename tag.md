---
layout: page
title: Tag
---
<div>
<h2>标签</h2>
<ul>
    {% for tag in site.tags %}
    	<li class="tag">
	<a href="#{{ tag[0] }}">
	<span>{{ tag[0] }}</span>
	<span>({{ tag[1] | size }}篇)</span>
	</a>
	</li>
    {% endfor %}
</ul>
<div>
<br>
<hr />
<br>
<div>
{% for tag in site.tags %}
        <h2 id="{{ tag[0] }}">
         {{ tag[0] }} 
        </h2>
        <ul>
                {% for post in tag[1] %}
                        <li class="file">
			{{ post.date | date:"%Y %b %d" }} - 
                        <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/'}}">
                             {{ post.title }}  
                        </a>
                        </li>
                {% endfor %}
        </ul>
{% endfor %}
<div>



