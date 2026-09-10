---
layout: page
permalink: /publications/
title: Publications
description: Publications grouped by research area, in reverse chronological order.
nav: true
nav_order: 3
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

<h2 class="bibliography">Anomaly Detection</h2>
{% bibliography --group_by none --query @*[topic=anomaly]* %}

<h2 class="bibliography">Applications of Generative Models</h2>
{% bibliography --group_by none --query @*[topic=generative]* %}

<h2 class="bibliography">Foundation Models</h2>
{% bibliography --group_by none --query @*[topic=foundation]* %}

<h2 class="bibliography">Other</h2>
{% bibliography --group_by none --query @*[topic=other]* %}

</div>
