---
layout: page
permalink: /readinglist/
title: reading list
description: Yearly reading list in reversed chronological order. 
years: [2021,2020]
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f readinglist -q @*[added={{y}}]* %}
{% endfor %}

</div>
