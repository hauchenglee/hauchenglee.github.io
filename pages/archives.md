---
layout: page
title: All articles are here
subtitle:
menu: archives
css: ['blog-page.css']
permalink: /archives.html
---

[//]: <> <span class="mega-octicon octicon-calendar"></span>&nbsp;&nbsp;專題系列： &nbsp;&nbsp; <a href ="http://www.hauchenglee.com/java.html"><font color="#1A0DAB">Java</font></a>&nbsp;&nbsp; <a href ="http://www.hauchenglee.com/life.html"><font color="#EB9439">Life</font></a>&nbsp;&nbsp; <a href ="https://www.pixiv.net/" target="_blank"><font color="#1E90FF">Pixiv</font></a>

<ul class="archives-list">
  {% for post in site.posts %}

    {% unless post.next %}
      <h3>{{ post.date | date: '%Y' }}</h3>
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        <h3>{{ post.date | date: '%Y' }}</h3>
      {% endif %}
    {% endunless %}

    <li><span>{{ post.date | date:'%m-%d' }}</span> <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>