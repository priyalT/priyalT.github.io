---
layout: default
title: Research
permalink: /research/
---

<div class="paper">
  <header>
    <h1 class="paper-heading">Research & Publications</h1>
    <p class="paper-intro">Official publications, conference presentations, and academic research projects spanning cancer genomics, machine learning, and transcriptomics.</p>
  </header>

  <ul class="entry-list">
    {% assign sorted_research = site.research_posts | sort: 'date' | reverse %}
    {% for item in sorted_research %}
      <li class="entry">
        <time class="entry-date">{{ item.date | date: "%b %d, %Y" }}</time>
        <div>
          <div style="display: flex; gap: 6px; align-items: center; margin-bottom: 6px; flex-wrap: wrap;">
            {% if item.type %}
              <span class="research-badge research-badge-type">{{ item.type }}</span>
            {% endif %}
            {% if item.venue %}
              <span class="research-badge research-badge-venue">{{ item.venue }}</span>
            {% endif %}
          </div>

          <div class="entry-title">
            <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
          </div>

          {% if item.authors %}
            <p class="entry-authors">
              <strong>Authors:</strong> {{ item.authors }}
            </p>
          {% endif %}

          {% if item.description %}
            <p class="entry-desc">{{ item.description }}</p>
          {% else %}
            <p class="entry-desc">{{ item.excerpt | strip_html | truncatewords: 25 }}</p>
          {% endif %}

          <div class="entry-links" style="margin-top: 10px;">
            <a class="research-link-pill" href="{{ item.url | relative_url }}">Read more...</a>
            {% if item.doi %}
              <a class="research-link-pill" href="https://doi.org/{{ item.doi }}" target="_blank" rel="noopener noreferrer">DOI ↗</a>
            {% endif %}
            <!-- {% if item.paper_url and item.paper_url != "" %}
              <a class="research-link-pill" href="{{ item.paper_url }}" target="_blank" rel="noopener noreferrer">Paper ↗</a>
            {% endif %} -->
            {% if item.code_url %}
              <a class="research-link-pill" href="{{ item.code_url }}" target="_blank" rel="noopener noreferrer">Code ↗</a>
            {% endif %}
          </div>


        </div>
      </li>
    {% else %}
      <li class="entry-empty" style="color: #64748b; font-style: italic;">No research published yet. Stay tuned!</li>
    {% endfor %}
  </ul>
</div>