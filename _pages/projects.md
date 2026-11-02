---
layout: page
title: projects
permalink: /projects/
description: A growing collection of your cool projects.
---

{% for project in site.projects %}
{% if project.redirect %}
  {% assign href = project.redirect %}
{% else %}
  {% assign href = project.url | prepend: site.baseurl | prepend: site.url %}
{% endif %}
<div class="project">
    <div class="thumbnail">
        <a href="{{ href }}"{% if project.redirect %} target="_blank"{% endif %}>
        <span>
            <h1>{{ project.title }}</h1>
            <p>{{ project.description }}</p>
        </span>
        </a>
    </div>
</div>
{% endfor %}
