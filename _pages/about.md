---
layout: about
title: About
permalink: /
subtitle:
order: 1

profile:
  align: right
  image: headshot_2024_cropped.jpg
  image_circular: false # crops the image to make it circular
  more_info: >
    <p>Fermilab, Wilson Hall 1167</p>
    <p>123 Batavia, IL</p>

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---
## I'm Oz, a Wilson Fellow & Associate Scientist at Fermilab working at the intersection of Machine Learning and Particle Physics

I'm a member of the CMS experiment at the Large Hadron Collider at CERN.
CMS studies the fundamental particles and forces which constitute all matter in the universe,
hoping to answer our deepest open questions about the fundamental nature of the universe.
We achieve this by colliding protons together at the highest energies possible,
and then looking through the millions of collisions produced every second for signs
of new fundamental interactions.
**I'm especially excited by the ways Machine Learning & AI can push
this science forward, letting us ask questions that simply weren't possible
before.**

I have a [blog on substack](https://ozamram.substack.com/) where you can read my marginally-filtered thoughts. 
I also used to write for [ParticleBites](https://www.particlebites.com/), summarizing recent
particle physics papers for a broad audience.

I completed my PhD at [Johns Hopkins University](https://jscholarship.library.jhu.edu/items/4e704274-b8f6-4199-845d-d8d7e3eb1fa7)
in 2022. 
I then joined Fermilab as a postdoc, and in 2026 became a Wilson Fellow (a
tenure-track associate scientist position). 

### A few things I work on

<div class="row row-cols-1 row-cols-md-3 g-4 mt-1 mb-4">
  <div class="col">
    <a href="{{ '/projects/#anomaly-detection' | relative_url }}">
      <div class="card h-100 hoverable">
        <img src="{{ '/assets/img/CASE_evt_display.png' | relative_url }}" class="card-img-top" style="height: 165px; object-fit: cover;" alt="Anomaly detection" />
        <div class="card-body">
          <h5 class="card-title">Anomaly Detection</h5>
          <p class="card-text">Model-agnostic searches that let the data itself flag unexpected new particles, instead of testing one theory at a time. I led the first such search at CMS.</p>
        </div>
      </div>
    </a>
  </div>
  <div class="col">
    <a href="{{ '/projects/#applications-of-generative-models' | relative_url }}">
      <div class="card h-100 hoverable">
        <img src="{{ '/assets/img/calo_challenge.jpg' | relative_url }}" class="card-img-top" style="height: 165px; object-fit: cover;" alt="Generative models" />
        <div class="card-body">
          <h5 class="card-title">Applications of Generative Models</h5>
          <p class="card-text">Harnessing modern generative AI --- diffusion models and normalizing flows --- to accelerate detector simulation and to perform high-dimensional, data-driven statistical inference.</p>
        </div>
      </div>
    </a>
  </div>
  <div class="col">
    <a href="{{ '/projects/#foundation-models' | relative_url }}">
      <div class="card h-100 hoverable">
        <img src="{{ '/assets/img/foundation_aoj_detector.png' | relative_url }}" class="card-img-top" style="height: 165px; object-fit: cover;" alt="Foundation models" />
        <div class="card-body">
          <h5 class="card-title">Foundation Models</h5>
          <p class="card-text">Training large models on real LHC collision data, and mapping out how they scale, as a foundation for many downstream physics tasks.</p>
        </div>
      </div>
    </a>
  </div>
</div>

You can find more on my [projects](/projects/) page.
