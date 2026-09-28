---
layout: default
title: Blog
permalink: /blog/
---

<div class="academic-container">

  <header class="blog-index-header">
    <h1>Blog & Notes</h1>
    <p>Thoughts on computational biology, bioinformatics pipelines, paper summaries, and machine learning.</p>
  </header>

  <div class="posts-list">
    {% for post in site.posts %}
      <article class="post-item">
        <time class="post-item-date">{{ post.date | date: "%b %d, %Y" }}</time>
        <div class="post-item-content">
          <h2 class="post-item-title">
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
          </h2>
          {% if post.description %}
            <p class="post-item-desc">{{ post.description }}</p>
          {% else %}
            <p class="post-item-desc">{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
          {% endif %}
          {% if post.tags %}
            <div class="post-item-tags">
              {% for tag in post.tags %}
                <span class="tag">#{{ tag }}</span>
              {% endfor %}
            </div>
          {% endif %}
        </div>
      </article>
    {% else %}
      <p style="color: #64748b; font-style: italic;">No posts published yet. Stay tuned!</p>
    {% endfor %}
  </div>

</div>