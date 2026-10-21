---
layout: page
title: AI Safety Projects
permalink: /ai-safety/
description: My current research in AI safety
nav: true
nav_order: 3
---

<!-- pages/ai_safety.md -->
<div class="projects">
  {% assign sorted_projects = site.projects | where: "category", "AI Safety" | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>
