---
layout: home
author_profile: true
entries_layout: grid
---

{% include feature_row id="intro" type="center" %}

<div class="portfolio-section">
  <h2>My Portfolio</h2>
  {% for item in site.portfolio %}
    <div class="portfolio-item">
      <h3><a href="{{ item.url }}">{{ item.title }}</a></h3>
      <p>{{ item.description }}</p>
      <!-- Optionally, include an image if your items have one -->
      {% if item.image %}
        <img src="{{ item.image }}" alt="{{ item.title }}" style="max-width: 100%; height: auto;">
      {% endif %}
    </div>
  {% endfor %}
</div>

{% include feature_row id="feature_row2" type="left" %}
