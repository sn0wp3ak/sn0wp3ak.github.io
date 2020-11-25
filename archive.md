---
layout: page
title: Archive
---

<section>
  {% if site.posts[0] %}

    {% capture currentyear %}{{ 'now' | date: "%Y" }}{% endcapture %}
    {% capture firstpostyear %}{{ site.posts[0].date | date: '%Y' }}{% endcapture %}
    {% if currentyear == firstpostyear %}
        <h2>今年的随笔</h2>
    {% else %}  
        <h2>{{ firstpostyear }}年的随笔</h2>
    {% endif %}
    
    {%for post in site.posts %}
      {% unless post.next %}
        <ul>
      {% else %}
        {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
        {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
        {% if year != nyear %}
          </ul>
          <h2>{{ post.date | date: '%Y' }}年的随笔</h2>
          <ul>
        {% endif %}
      {% endunless %}
        <li style="list-style-image:url('../assets/paper.png');"><time>{{ post.date | date:"%b %d" }} - </time>
          <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
            {{ post.title }}
          </a>
        </li>
    {% endfor %}
    </ul>

  {% endif %}
</section>
