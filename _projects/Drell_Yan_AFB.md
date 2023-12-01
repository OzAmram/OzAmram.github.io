---
layout: page
title: Parity Violation
description: Measuring the Drell-Yan forward-backward asymmetry with CMS
img: /assets/img/rose_mirror_parity_violation.jpg
importance: 2
category: work
giscus_comments: false
related_publications: CMS:2022uul
---
<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/rose_mirror_parity_violation.jpg' | relative_url }}" alt="" title="CMS Collisions"/>
    </div>
</div>
<div class="caption">
    Physics doesn't work the same in the mirror!
</div>

When you look at yourself in the mirror, you appear the same except all of your features are 'flipped'. 
Similarly, your right hand and your left hand are mirror version of each, you
can't rotate one to match the other, but if you mirrored one it would.
The name for this mathematical symmetry is called 'parity'.
In the 1950's it was discovered that shockingly the laws of physics violate
parity!
In a famous experiment [Chien-Shiung Wu](https://en.wikipedia.org/wiki/Chien-Shiung_Wu) showed that weak nuclear force treats, 'mirrored' versions of particles differently than regular ones. 
At the time, this was quite surprising, but now parity violation in weak force
is key component of the Standard Model of particle physics. 

In this project I used CMS data to measure an observable called the 'forward-backward asymmetry' that quantifies 
the amount of parity violation in the process of a quark-antiquark annihilating to produce a pair of leptons.
The the forward-backward asymmetry is an asymmetry in angular distribution of
the out going leptons. Because the weak force violates parity a lot, this
asymmetry is quite large and its expected value in the Standard Model can be
computed quite well. 
By comparing our measurements of this observable to predictions from the standard model, we can look for hints of yet-undiscovered particles that change the amount of parity violation in this process. 

You might wonder why this asymmetry is a good place to look for the presence of new
particles. 
The most common way to look for new particles is called a 'bump-hunt', in
which you look at the invariant mass distribution of your final state
particles. 
If you were sometimes producing a new particle in the collision that was
decaying into your final state particles, you would see
an excess of events (a bump) at the invariant mass distribution at the mass of
that new particle. This is [how the Higgs boson was discovered](https://atlas.cern/updates/briefing/exploring-higgs-discovery-channels) in 2012.
However, if your bump gets too wide (because the new particle you are producing
as a large 'decay width') it can be very easy to miss, as the bump starts to
blend in with the smooth background distribution. 
The other limitation of the bump-hunt is that it quickly becomes limited by the
energy of the collider you are using. If the mass of a hypothetical new particle is too
large compared to the energy of the collider you won't be able to
produce it directly and see a bump. 
In contrast, measurements of an angular asymmetry are very insensitive to the
width of the new particle being produced, so it will not miss large-width
particles. 
Additionally, an asymmetry measurement can reveal the precense of new particles through interference with known processes
rather than their direct production. Even if you don't have
enough energy to produce a new particle directly, it still can produce subtle
effects that alter processes at lower energies through '[virtual](https://en.wikipedia.org/wiki/Virtual_particle)' contributions. 
It is much easier to spot these sort of interference effects with an angular
asymmetry because you are much less sensitive to experimental uncertainties
(like the efficiency of your detector at very high energies) than measurements
of the invariant mass distribution. 

<div class="row  justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/AFB_SM_Zprime.png' | relative_url }}" width = "600" alt="" title="Predictions for the forward-backward asymmetry"/>
    </div>
</div>
<div class="caption">
    Predictions for the forward-backward asymmetry (A<sub>FB</sub>) as a function of mass. One can see
    that heavy new Z' of a mass of 3 TeV can already produce notable
    distortions from the standard model (SM) at 1 TeV. 
</div>


Our measurement of the forward-backward asymmetry  (A<sub>FB</sub>) uses a new 'template fitting' technique which allows us to
fit the observed angular distribution to templates derived from simulated Monte Carlo events in
order to extract the parton-level asymmetry. 
This improves the accuracy and interpretability as compared to the counting
method used in previous LHC measurements. 
The asymmetry is measured as a function of mass for dielectron and dimuon pairs
with mass greater than 170 GeV. We are able to measure A<sub>FB</sub> with
a precision of 2% in the lower mass bins, and around 10% for the masses above
1 TeV. 
When combining the electron and muon channels, the results agree quite well
with the standard model prediction, and the measurement is used rule out the
existence of new heavy Z's with masses less than 4.4 TeV. 


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/AFB_mbins_unblind.png' | relative_url }}" alt="" title="Results of AFB measurement"/>
    </div>
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/limit_obs.png' | relative_url }}" alt="" title="Limits on the existence of a new Z' based on the measurment."/>
    </div>
</div>
<div class="caption">
Left, the results of the measurement as compared to standard model predictions.
Right, limits on the existence of a Z' based on the measurement. 
</div>


However, you might notice in the plot above that the electron and muon values
seem to differ slightly at lower masses. 
In the standard model they should agree exactly, but a new particle that only
interacted with electrons or muons and not the other could produce such an
effect.
We also make a dedicated measurement of the difference in asymmetry between
electrons and muons (which has further reduced uncertainties) and find
they differ with a significance of 2.4-sigma.  Is this a statistical fluctuation, or possibly a hint of
something related to the [recent lepton flavor anomalies](https://cerncourier.com/a/flavour-anomalies-continue-to-intrigue/)? More data
will tell!


<div class="row  justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/delta_AFB.png' | relative_url }}" width = "700" alt="" title="Measurement of the difference between the electron and muon asymmetries"/>
    </div>
</div>
<div class="caption">
Measurement of the difference between the electron and muon asymmetries as
a function of mass. In the standard model the difference should be zero, 
but when combining the full mass range, we observe a difference between the two
at the level of 2.4-sigma. 
</div>

Because all of the measurements have very small systematic uncertainties, the precision will continue to improve with more data.
This means that the sensitivity to heavy new heavy gauge bosons ( Z' s) will over take that of the
direct bump-hunt searches in the future.

For more details, you can check out the
[paper](https://arxiv.org/abs/2202.12327).
CMS members can checkout the CADI line [SMP-21-002](https://cms.cern.ch/iCMS/analysisadmin/cadilines?line=SMP-21-002&tp=an&id=2415&ancode=SMP-21-002).
