---
layout: default
title: Blog
permalink: /blog/
---

<div class="paper">

  <h1 class="paper-heading">Blog</h1>
  <p class="paper-intro">Notes and tutorials on computational biology, bioinformatics pipelines, tools I've been building, and research I find interesting.</p>

  <ul class="entry-list">
    {% for post in site.posts %}
      <li class="entry">
        <span class="entry-date">{{ post.date | date: "%b %-d, %Y" }}</span>
        <div>
          <a class="entry-title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
          {% if post.description %}
            <p class="entry-desc">{{ post.description }}</p>
          {% endif %}
        </div>
      </li>
    {% else %}
      <li class="entry-empty">Nothing here yet.</li>
    {% endfor %}
  </ul>

</div>
