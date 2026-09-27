---
layout: default
title: Projects
---

<h1>Projects</h1>

<div class="projects-grid">

{% for project in site.projects %}

  <article class="project-card">

    <p class="eyebrow">PROJECT</p>

    <h2>
      <a href="{{ project.url }}">
        {{ project.title }}
      </a>
    </h2>

    <p>
      {{ project.description }}
    </p>

    <div class="tags">
      {% for tag in project.tags %}
        <span>{{ tag }}</span>
      {% endfor %}
    </div>

  </article>

{% endfor %}

</div>