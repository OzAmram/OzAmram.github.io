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
## I'm Oz --- an ML + physics researcher at Fermilab, moving into AI safety

I'm a Wilson Fellow and Associate Scientist at Fermilab and a member of the CMS
experiment at the Large Hadron Collider. For the past several years my research
has been on machine learning for particle physics: anomaly detection, generative
models, and foundation models, mostly aimed at finding things in complex collider data
that nobody knew to look for.

**I am now transitioning to AI safety research, and am actively looking for
roles in the field.**

The rapid increase in AI capabilities over the last year, and recent public misalignment incidents have convinced me AI safety is an urgent issue,
and worth leaving my current research and tenure-track position behind for.
I believe I have the technical skills, research history and motivation 
to be effective in alignment or interpretability research roles. 

### What carries over

The problems are not the same, but a lot of the machinery is.

**Finding behaviour nobody specified in advance.** Anomaly detection has been my biggest research focus of the past few years:
searching for new particles in complex, massive datasets from the LHC, with no labelled examples.
I have developed and used weakly supervised and unsupervised methods ML methods for this task, and
established best practices of how such methods should be evaluated and validated. 
I led the first such search at CMS, running five complementary methods over 30
million collision events.

**Knowing when a model can be trusted off-distribution.** Classifiers in particle
physics are often trained on simulation, but deployed on real data. 
And the gap between the two is hugely important. I developed a method CMS now uses as standard for
calibrating that gap and putting defensible uncertainties on it.

**Saying why a model flagged something.** For that search I also built the
interpretability framework --- characterising what made a flagged event anomalous
and mapping it back to physical detector signatures. It was the first for an
anomaly search at the LHC.

**Evaluating generative models honestly.** I wrote
[CaloDiffusion](/projects/CaloDiffusion.html) from scratch, and now lead the
effort benchmarking generative models for the CMS calorimeter upgrade. Most of
that work is the evaluation framework, building quantitative metrics that capture 
how closely the model is matching physics-based simulations, and highlighting
which features are mismodeled. 

My [résumé](/resume/) is a two-page summary aimed
at AI safety roles. My [full CV](/cv/) has the complete record.

### The physics

CMS studies the fundamental particles and forces that make up all matter in the
universe. We collide protons at the highest energies we can reach and sift
through the millions of collisions produced every second for signs of new
interactions. 
My research has focused on applying novel machine learning methods to the analysis of this data. 

I completed my PhD in physics at [Johns Hopkins University](https://jscholarship.library.jhu.edu/items/4e704274-b8f6-4199-845d-d8d7e3eb1fa7)
in 2022, joined Fermilab as a postdoc, and in 2026 became a Wilson Fellow (tenure-track assistant professor equivalent).

A few of the things I've worked on:

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

I have a [blog on substack](https://ozamram.substack.com/) where you can read my
marginally-filtered thoughts. I also used to write for
[ParticleBites](https://www.particlebites.com/), summarizing recent particle
physics papers for a broad audience.

