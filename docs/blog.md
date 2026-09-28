---
layout: default
title: Blog
permalink: /blog/
---

<div class="paper">
  <header>
    <h1 class="paper-heading">Blog & Notes</h1>
    <p class="paper-intro">Thoughts on computational biology, bioinformatics pipelines, paper summaries, and machine learning.</p>
  </header>

  <ul class="entry-list">
    {% for post in site.posts %}
      <li class="entry">
        <time class="entry-date">{{ post.date | date: "%b %d, %Y" }}</time>
        <div>
          <div class="entry-title">
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          </div>
          {% if post.description %}
            <p class="entry-desc">{{ post.description }}</p>
          {% else %}
            <p class="entry-desc">{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
          {% endif %}

        </div>
      </li>
    {% else %}
      <li class="entry-empty" style="color: #64748b; font-style: italic;">No posts published yet. Stay tuned!</li>
    {% endfor %}
  </ul>
</div>