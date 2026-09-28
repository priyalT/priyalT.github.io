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

            
        </div>
      </li>
    {% else %}
      <li class="entry-empty">Nothing here yet.</li>
    {% endfor %}
  </ul>

</div>
