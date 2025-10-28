---
layout: default
title: Home
---
## Hello,

<div class="about-card">
  <img src="/assets/images/resume-photo.jpg" alt="Hector Jimenez" class="about-photo" style="width:140px;height:140px;border-radius:50%;float:right;margin:0 0 1rem 1rem;">

  <p>
  I’m Hector Jimenez, an Electrical Engineer specializing in control systems and circuit design. I design and prototype embedded hardware (STM32, ESP32), perform PCB layout in KiCad, and develop firmware in C/C++ for data acquisition and control. I have hands-on experience with Hardware Design, PLC systems testing (ABB AC800M), HMI development, RF instrument calibration, and power electronics modeling (LTSpice).
  </p>

  <ul>
    <li>Email: <a href="mailto:hectorjimenezuc@gmail.com">hectorjimenezuc@gmail.com</a></li>
    <li>Phone: (657) 293-5388</li>
    <li>Availability: M-F (1-5 PM PST)</li>
    <li>GitHub: <a href="https://github.com/xmyoot" target="_blank" rel="noopener">xmyoot</a></li>
    <li>LinkedIn: <a href="https://www.linkedin.com/in/hectorjimenezz" target="_blank" rel="noopener">hectorjimenezz</a></li>
    <li>CV: <a href="/assets/resume.pdf" target="_blank" rel="noopener">Download resume</a></li>
  </ul>
</div>

---

## Projects

{% if site.projects == empty %}
<p>No projects yet — add files in <code>_projects/</code>.</p>
{% endif %}

<div class="projects-grid">
{% assign projects = site.projects | sort: 'date' | reverse %}
{% for project in projects %}
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