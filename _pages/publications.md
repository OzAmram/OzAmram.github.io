---
layout: page
permalink: /publications/
title: Publications
description: Publications grouped by research area, in reverse chronological order.
nav: true
nav_order: 2
---

<!-- _pages/publications.md -->

_A note on author lists: Much of my work was done as part of the CMS experiment which has thousands of members.
CMS publishes ~100 papers per year, and every collaboration member is listed as an author on every CMS paper.
As a result, my Google Scholar and Inspire profiles include hundreds of collaboration papers I had little direct contribution to.
This page lists only the papers I made a substantial contribution to._
{: style="font-size: 0.85rem; margin-bottom: 2.5rem;"}



<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

<h2 class="bibliography">AI Safety</h2>
{% bibliography --group_by none --query @*[topic=safety]* %}

<h2 class="bibliography">Physics: Anomaly Detection</h2>
{% bibliography --group_by none --query @*[topic=anomaly]* %}

<h2 class="bibliography">Physics: Applications of Generative Models</h2>
{% bibliography --group_by none --query @*[topic=generative]* %}

<h2 class="bibliography">Physics: Foundation Models</h2>
{% bibliography --group_by none --query @*[topic=foundation]* %}

<h2 class="bibliography">Physics: Other</h2>
{% bibliography --group_by none --query @*[topic=other]* %}

</div>
