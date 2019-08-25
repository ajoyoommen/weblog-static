---
layout: default
title: "Ajoy Oommen's Web Log"
is_home_page: true
---
{% for post in site.posts %}
  <h3>
    <span class="font-monaco font-size-70pc">
      {{ post.date | date_to_string }} &raquo; </span>
    <a href="{{ post.url  }}">
      {{ post.title }}</a>
  </h3>
{% endfor %}
