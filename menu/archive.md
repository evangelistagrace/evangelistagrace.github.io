---
layout: page
title: Archive
---
<ul class="posts">
  {% for post in site.posts %}

    {% unless post.next %}
      <h3 class="dateStyle">{{ post.date | date: '%B' }}, {{ post.date | date: '%Y' }}</h3>
    {% else %}
      {% capture month %}{{ post.date | date: '%B' }}{% endcapture %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nmonth %}{{ post.next.date | date: '%B' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      
      {% if month != nmonth %}
      <br>
        <h3 class="dateStyle">{{ post.date | date: '%B' }}, {{ post.date | date: '%Y' }}</h3>
      {% endif %}
    {% endunless %}

    <li itemscope>
      <a href="{{ site.github.url }}{{ post.url }}">{{ post.title }}</a>
    </li>

  {% endfor %}
</ul>
