---
layout: page
permalink: /nextup/
title: next up
description: The next set of books to read.
years: [2021,2020]
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f toread -q @*[added={{y}}]* %}
{% endfor %}

</div>
