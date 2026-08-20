---
layout: page
title: Projects
permalink: /projects/
description: Some things I have worked on
nav: true
nav_order: 3
horizontal: false
order: 2
display_categories:
  - Anomaly Detection
  - Applications of Generative Models
  - Foundation Models
  - Older Projects

category_descriptions:
  Anomaly Detection: >
    Model-agnostic searches that let the data itself flag new phenomena,
    broadening our sensitivity to the unexpected.
  Applications of Generative Models: >
    Using diffusion models and normalizing flows to accelerate detector
    simulation and to perform high-dimensional, data-driven inference.
  Foundation Models: >
    Training large models on real LHC collision data, as a basis for many
    downstream physics tasks.
  Older Projects: >
    Earlier work on detector calibration and precision electroweak
    measurements.
---

<!-- pages/projects.md -->
<div class="projects">
{% if site.enable_project_categories and page.display_categories %}
  <!-- Display categorized projects -->
  {% for category in page.display_categories %}
  <a id="{{ category | slugify }}" href=".#{{ category | slugify }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign category_blurb = page.category_descriptions[category] %}
  {% if category_blurb %}
    <p class="category-description">{{ category_blurb }}</p>
  {% endif %}
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal %}
  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
  {% endfor %}

{% else %}

<!-- Display projects without categories -->

{% assign sorted_projects = site.projects | sort: "importance" %}

  <!-- Generate cards for each project -->

{% if page.horizontal %}

  <div class="container">
    <div class="row row-cols-1 row-cols-md-2">
    {% for project in sorted_projects %}
      {% include projects_horizontal.liquid %}
    {% endfor %}
    </div>
  </div>
  {% else %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
  {% endif %}
{% endif %}
</div>
