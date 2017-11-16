---
layout: archive
title: "Media Gallery"
date: 2014-05-30T11:40:45-04:00
modified:
excerpt: "Some pencil sketches, paintings and artwork! (a simple gallery)"
tags: []
image:
  feature:
  teaser:
---

<div class="tiles">
{% for post in site.categories.media %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->