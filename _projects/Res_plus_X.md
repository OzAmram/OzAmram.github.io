---
layout: page
title: Di-Object + X
description: Unearthing a new class of anomaly detection signatures
img: /assets/img/diobject_plus_x_thumbnail.png
importance: 3
category: Anomaly Detection
giscus_comments: false
related_publications: true
---

The [dijet anomaly search](/projects/CASE.html) showed that anomaly detection
can work on real CMS data. But it only targeted one class of anomalies: 
dijet resonances where the 'anomalousness' lives in the substructure of the two big jets.

That is a reasonable place to start, because jets are the most common thing the
LHC produces and their substructure is a rich, high-dimensional object to hunt
through. But there are plenty of models of new physics that could show up in totally different final states. 
To ensure we aren't missing anything in LHC data, we need to take the tools we pioneered in the first CMS dijet search 
and apply them to a whole program of anomaly detection searches covering as many signatures as possible. 

One natural starting point is to expand beyond anomalies showing up in jet substructure. Plenty of models
don't put their strangeness inside a jet at all --- they put it in the *rest of
the event*.

### Why dark sectors break the usual search strategy

Dark sector models are one of the main signals motivating this work: hypothetical
particles that talk to the Standard Model only feebly, often through a whole
hidden family of states rather than a single new particle. A characteristic
feature of these models is that they don't decay in one step. A heavy state
decays to a lighter one, which decays to a lighter one still, and only at the
end of the chain does something visible pop out. The result is an event with a
long tail of extra activity: extra jets, missing energy, soft leptons,
objects at odd angles.

This is exactly the kind of thing that falls through the cracks. A traditional
search picks one decay chain, simulates it, and optimizes for it. If the chain
has three or four steps, the number of possible variations explodes, and there
is no realistic hope of covering them one at a time. Meanwhile a plain
'bump hunt' --- looking for a mass peak in some pair of particles --- throws away
all of the extra activity that would have made the event stand out, meaning signals can
stay buried in the background and be missed by our searches.

### Di-object + X

The proposal is a topology we call **di-object plus X**. You look for a
resonance decaying to two ordinary Standard Model particles, produced alongside
other event activity, and you let that extra activity --- the X --- be the thing you use
to detect anomalies.

<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/diobject_plus_x_thumbnail.png' | relative_url }}" alt="" title="The di-object plus X topology" width="500"/>
    </div>
</div>
<div class="caption">
    The di-object plus X topology. A resonance decays to two Standard Model
    objects --- a pair of taus or muons --- which gives us a mass spectrum to do
    statistics with. Everything else the collision produced is X, and that is
    what the anomaly classifier is trained on.
</div>

The two-object resonance is what gives you a handle for the statistics: it
provides a mass spectrum where a signal would show up as a localized bump, and
sidebands around that bump give you a data-driven estimate of the background.
That's the same [CATHODE](https://arxiv.org/abs/2109.00546) methodology used in the dijet search: a generative model is trained in
the resonance sidebands, a potentially signal-enriched region is compared against background events
drawn from that generative model, and a classifier is trained to tell them apart, entirely in data. The difference is
what the classifier gets to look at. Instead of jet substructure, it sees the
whole event.

We studied this in the di-$$\tau$$ and di-$$\mu$$ final states, where the two
taus or two muons form the resonance and everything else is X.

### Describing a whole event to a neural network

Feeding 'the rest of the event' to a network is harder than it sounds. Events
have variable numbers of objects, no canonical ordering, and the obvious
variables one might reach for tend to be strongly correlated with the resonance
mass --- which is fatal here, because a classifier that secretly learns the mass
will sculpt fake bumps into the background.

So instead of hand-picking variables, we use a set derived from the geometry of
the collision's phase space. Any $$N$$-body final state can be described by
coordinates on a simplex, built out of the final-state momenta as

$$\rho_i = \frac{p_{T,i}}{Q} e^{\pm y_i}$$

where $$Q$$ is the overall energy scale of the event, and $$p_T$$ and $$y$$ are
the transverse momentum and rapidity of each object. These are the natural
coordinates for describing how a collision's energy is distributed, and we found them
to work better than simple momenta and angular variables.

### Does it work?

We tested this on three benchmark signals, all of which hide their new physics
in the extra activity rather than in the resonance itself: a heavy Higgs
produced alongside top quarks ($$t\bar{t}\phi$$), a pair of vector-like quarks
decaying in two steps through an intermediate scalar
($$T'\bar{T'} \to t S^0 \bar{t} S^0$$), and an NMSSM-style cascade
$$X \to YH$$ where the $$H$$ goes invisible and shows up only as missing energy.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/Res_plus_X_signif_mu.png' | relative_url }}" alt="" title="Sensitivity enhancement, di-muon channel" width="500"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/Res_plus_X_signif_tau.png' | relative_url }}" alt="" title="Sensitivity enhancement, di-tau channel" width="500"/>
    </div>
</div>
<div class="caption">
    Sensitivity enhancement in the di-muon (left) and di-tau (right) channels.
    The horizontal axis is how significant the signal looks before any anomaly
    detection selection; the vertical axis is what you get after cutting on the
    anomaly score. The dashed line marks the 5-sigma discovery threshold. Bands
    show the spread over five independent training runs.
</div>

This is the plot I'd point at to summarize the whole thing. The horizontal axis
is essentially what a conventional bump hunt would report --- the significance
of the signal before the anomaly detection does anything. The vertical axis is
what you get afterwards. A signal sitting at a few tenths of a standard
deviation, the sort of wiggle nobody would look at twice, ends up over the
$$5\sigma$$ discovery threshold. For the vector-like quark in the di-$$\mu$$
channel that happens at around $$0.35\sigma$$ of injected signal; the other
models need somewhat more, and the di-$$\tau$$ channel is harder than di-$$\mu$$
across the board, which is unsurprising given how much messier hadronic taus are
to reconstruct.

### What's next

This paper is a proof of concept, done outside the experiment. The real test, as
always, is running it on actual collision data and getting it through collaboration review.
A CMS search using this strategy is in progress, stay tuned for the result!

This work was primarily led by UCSB graduate student Liam Brennan, so all credit to him for taking the idea I had to perform anomaly detection in this topology and actually making it work!
It's been a fun experience to see this project come to fruition in a purely 'advisory' role, without ever seeing the code. I guess we all become project managers at some point!

Our phenomenological study is described in {% cite Res_plus_X %}.
CMS members can see more about our analysis [here](https://cmsfence.cern.ch/alcm/cmsanalysis/details/ancode=EXO-26-006).