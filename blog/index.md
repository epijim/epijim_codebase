---
layout: archive
title: "My blog"
date: 2014-05-30T11:40:45-04:00
modified:
excerpt: "A data driven take on my life."
tags: []
image:
  feature:
  teaser:
---

<div class="tiles">
{% for post in site.categories.blog %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->