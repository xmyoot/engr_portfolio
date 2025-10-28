---
layout: page
title: Projects
permalink: /projects/
---
<ul>
{% for project in site.projects | sort: 'date' | reverse %}
  <li><a href="{{ project.url | relative_url }}">{{ project.title }}</a> — {{ project.excerpt }}</li>
{% endfor %}
</ul>
