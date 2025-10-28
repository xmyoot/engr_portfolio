---
layout: default
title: Home
---
## About

I’m an Electrical Engineer specializing in hardware design, testing, and embedded systems. My work focuses on creating reliable, efficient circuits and bringing ideas from schematic to prototype. I’m passionate about open-source hardware and software, and I enjoy contributing to projects that make engineering tools and knowledge more accessible to everyone.

- Email: hectorjimenezuc@gmail.com  
- GitHub: xmyoot
- CV: Link

---

## Projects

{% if site.projects == empty %}
<p>No projects yet — add files in <code>_projects/</code>.</p>
{% endif %}

<div class="projects-grid">
{% for project in site.projects %}
  <article class="project-card">
    <a class="project-link" href="{{ project.url | relative_url }}">
      {% if project.thumbnail %}
        <div class="thumb-wrap">
          <img src="{{ project.thumbnail | relative_url }}" alt="{{ project.title }} thumbnail" class="project-thumb"/>
        </div>
      {% endif %}
      <div class="project-body">
        <h3 class="project-title">{{ project.title }}</h3>
        <p class="project-excerpt">{{ project.excerpt | default: project.content | strip_html | truncate: 120 }}</p>
      </div>
    </a>
  </article>
{% endfor %}
</div>