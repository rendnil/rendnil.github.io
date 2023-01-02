---
layout: default
title: Archive
---
# Archive
<!-- <br/>
{% assign postsByYearMonth = site.posts | group_by_exp: "post", "post.date | date: '%B %Y'" %}
{% for yearMonth in postsByYearMonth %}
  <h2>{{ yearMonth.name }}</h2>
  <ul>
  
    {% for post in yearMonth.items %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %} -->

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
<ul style="list-style: none; padding-left:0">
{% for post in sorted_posts %}
    <li class="archive-bullet"> -- <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>
