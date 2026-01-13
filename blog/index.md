---
layout: post
title: Blog
---
{% assign year = "" %}

{% for post in site.posts %}
  {% capture y %}{{ post.date | date: "%Y" }}{% endcapture %}

  {% if year != y %}
    {% if year != "" %}
      </ul>
    {% endif %}
    {% assign year = y %}
    <h2 class="listing-year">{{ y }}</h2>
    <ul class="listing">
  {% endif %}

  <li class="listing-item">
    <time datetime="{{ post.date | date: "%Y-%m-%d" }}">
      {{ post.date | date: "%Y-%m-%d" }}
    </time>
    <a href="{{ post.url | prepend: site.baseurl }}" title="{{ post.title }}">
      {{ post.title }}
    </a>
  </li>
{% endfor %}
</ul>
