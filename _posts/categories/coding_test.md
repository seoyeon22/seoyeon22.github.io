---
title: "Coding Test"
layout: archive
permalink: categories/coding_test
author_profile: true
types: posts
---

{% assign posts = site.categories['coding_test']%}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}