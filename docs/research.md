---
layout: default
title: Research
permalink: /research/
---

<div class="academic-container">

  <header class="research-index-header">
    <h1>Research & Publications</h1>
    <p>Official publications, conference presentations, and academic research projects spanning cancer genomics, machine learning, and transcriptomics.</p>
  </header>

  <div class="research_post-list">
    {% assign sorted_research = site.research_posts | sort: 'date' | reverse %}
    {% for item in sorted_research %}
      <article class="research_post-item">
        <time class="research_post-item-date">{{ item.date | date: "%b %d, %Y" }}</time>
        <div class="research_post-item-content" style="flex: 1;">
          
          <div style="display: flex; gap: 6px; align-items: center; margin-bottom: 6px; flex-wrap: wrap;">
            {% if item.type %}
              <span class="research-badge research-badge-type">{{ item.type }}</span>
            {% endif %}
            {% if item.venue %}
              <span class="research-badge research-badge-venue">{{ item.venue }}</span>
            {% endif %}
          </div>

          <h2 class="research_post-item-title">
            <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
          </h2>

          {% if item.authors %}
            <p style="font-size: 13px; color: #64748b; margin: 0 0 6px 0;">
              <strong>Authors:</strong> {{ item.authors }}
            </p>
          {% endif %}

          {% if item.description %}
            <p class="research_post-item-desc">{{ item.description }}</p>
          {% else %}
            <p class="research_post-item-desc">{{ item.excerpt | strip_html | truncatewords: 25 }}</p>
          {% endif %}

          <div class="research-links">
            <a class="research-link-pill" href="{{ item.url | relative_url }}">
              Read Overview →
            </a>
            {% if item.doi %}
              <a class="research-link-pill" href="https://doi.org/{{ item.doi }}" target="_blank" rel="noopener noreferrer">
                DOI ↗
              </a>
            {% endif %}
            {% if item.paper_url and item.paper_url != "" %}
              <a class="research-link-pill" href="{{ item.paper_url }}" target="_blank" rel="noopener noreferrer">
                Paper ↗
              </a>
            {% endif %}
            {% if item.code_url %}
              <a class="research-link-pill" href="{{ item.code_url }}" target="_blank" rel="noopener noreferrer">
                Code ↗
              </a>
            {% endif %}
          </div>

          {% if item.tags %}
            <div class="research_post-item-tags" style="margin-top: 10px;">
              {% for tag in item.tags %}
                <span class="tag">#{{ tag }}</span>
              {% endfor %}
            </div>
          {% endif %}
        </div>
      </article>
    {% else %}
      <p style="color: #64748b; font-style: italic;">No research published yet. Stay tuned!</p>
    {% endfor %}
  </div>

</div>