---
layout: default
---
<p class="tagline">{{ site.description }}</p>

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    {% if post.description %}
    <br><span class="post-desc">{{ post.description }}</span>
    {% endif %}
  </li>
{% endfor %}
</ul>


