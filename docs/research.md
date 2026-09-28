---
layout: default
title: Research
permalink: /research/
---

<div class="paper">

  <h1 class="paper-heading">Research</h1>
  <p class="paper-intro">Publications, conference presentations, and research projects in cancer genomics, transcriptomics, and machine learning.</p>

  <ul class="entry-list">
    {% assign sorted_research = site.research_posts | sort: 'date' | reverse %}
    {% for item in sorted_research %}
      <li class="entry">
        <span class="entry-date">{{ item.date | date: "%Y" }}</span>
        <div>
          <a class="entry-title" href="{{ item.url | relative_url }}">{{ item.title }}</a>
          {% if item.authors %}
            <p class="entry-authors">{{ item.authors | replace: "Priyal Tripathi", "<strong>Priyal Tripathi</strong>" }}</p>
          {% endif %}
          <p class="entry-venue">
            {% if item.venue %}<em>{{ item.venue }}</em>{% endif %}{% if item.venue and item.type %}. {% endif %}{{ item.type }}
          </p>
          <p class="entry-links">
            <a href="{{ item.url | relative_url }}">summary</a>
            {% if item.paper_url and item.paper_url != "" %} · <a href="{{ item.paper_url }}" target="_blank" rel="noopener noreferrer">paper</a>{% endif %}
            {% if item.doi %} · <a href="https://doi.org/{{ item.doi }}" target="_blank" rel="noopener noreferrer">doi</a>{% endif %}
            {% if item.code_url %} · <a href="{{ item.code_url }}" target="_blank" rel="noopener noreferrer">code</a>{% endif %}
          </p>
        </div>
      </li>
    {% else %}
      <li class="entry-empty">Nothing here yet.</li>
    {% endfor %}
  </ul>

</div>
