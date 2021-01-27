---
layout: default
title: Archive
---

<section>
  {% if site.posts[0] %}

    {% capture currentyear %}{{ 'now' | date: "%Y" }}{% endcapture %}
    {% capture firstpostyear %}{{ site.posts[0].date | date: '%Y' }}{% endcapture %}
    {% if currentyear == firstpostyear %}
        <h2>今年的笔记</h2>
	<hr>
    {% else %}  
        <h2>{{ firstpostyear }}年的笔记</h2>
	<hr>
    {% endif %}
    
    {%for post in site.posts %}
      {% unless post.next %}
        <ul>
      {% else %}
        {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
        {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
        {% if year != nyear %}
          </ul>
          <h2>{{ post.date | date: '%Y' }}年的笔记</h2>
	  <hr>
          <ul>
        {% endif %}
      {% endunless %}
        <li class="file"><time>{{ post.date | date:"%b %d" }} - </time>
          <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
            {{ post.title }}
          </a>
        </li>
    {% endfor %}
    </ul>

  {% endif %}
</section>
