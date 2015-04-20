---
layout: archive
title: "Code"
date: 2014-09-30T11:39:03-04:00
modified:
excerpt: "My favourite pieces of analysis code."
tags: [code]
image:
  feature:
  teaser:
---

<div class="tiles">
{% for post in site.categories.code %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->