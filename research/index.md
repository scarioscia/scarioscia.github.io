---
layout: post
title: Research
---

<ul class="listing">
  {% for research_post in site.research_posts %}
    <li class="listing-item">
      <a href="{{ research_post.url | prepend: site.baseurl }}" title="{{ research_post.title }}">
        <img src="{{ site.baseurl }}{{ research_post.icon }}" alt="{{ research_post.title }} Icon" /> <!-- Use the icon specified in the front matter -->
        <br>
        {{ research_post.title }}
      </a>
    </li>
  {% endfor %}
</ul>

