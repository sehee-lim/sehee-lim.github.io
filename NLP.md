---
layout: default
title: "NLP"
---

<h1>{{ page.title }}</h1>

{% for post in site.categories.NLP %}
  <a href="{{ post.url }}">{{ post.title }}</a><br>
{% endfor %}