---
layout: page
title: CMS Dijet Resonance Anomaly Search
description: The first application of anomaly detection in CMS
img: /assets/img/CASE_evt_display.png
importance: 1
category: work
giscus_comments: false
related_publications: CMS:CASE
---
<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/CASE_evt_display.png' | relative_url }}" alt="" title="CMS Collisions" width="800"/>
    </div>
</div>
<div class="caption">
An 'anomalous' CMS event selected by our unsupervised AI algorithm. 
</div>

LHC experiments spend a lot of time searching through their data for signs of
new particles. 
If new particles exist, they are likely produced extremely rarely, and may
leave signatures very similar to those of much high rate background processes. 
So in order to perform searches, analyzers have to carefully design
a statistical analysis to target a particular signal.
Such analyses often have great sensitivity to the particular particle being
searched for, but very poor sensitivity to anything else.
While this approach was a spectular success in the case of an 'expected' new
particle like the Higgs, one may wonder if we could be missing 'unexpected' new particles
because we haven't thought to (or had time to) search for them.

This is the gap in coverage that 'anomaly detection' tries to solve.
The strategy is use novel ML methods to cast a wider net that would otherwise
be possible, making sure we are prepared for the unexpected. 
[Tag N' Train](projects/TagNTrain), one of my projects from grad school, was 
one such type of anomaly detection algorithm. 
We showed it could be successfully find signals in simulated datasets and data
challenges. But of course the real challenge is to actually apply it to real
CMS data (and get through internal CMS review).
Well after many years of effort the results are here.

<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/CASE_jet_diagram.png' | relative_url }}" alt="" title="Cartoon of dijet anomaly signature" width="800"/>
    </div>
</div>
<div class="caption">
    A cartoon of the type of signature we are looking for : some heavy new particle A decays into two other particles, B and C which produce jets.
</div>


The title of the paper is '[Model-agnostic search for dijet resonances with anomalous jet substructure in proton-proton collisions at sqrt{s} = 13 TeV](https://cms-results.web.cern.ch/cms-results/public-results/preliminary-results/EXO-22-026/index.html)'.
'Jets' are colimnated sprays of particles that show up in our detector
originating from particles in the collision that interact with the strong nuclear force (ie quarks or gluons). 
They are the most common type of signature produced in a proton-proton collider
like the LHC, which means searches for new particles producing jets will have
large backgrounds that limit their sensitivity.
While jets from different types of particles look the same on the surface, they
can differ in terms of their 'substructure', or how the energy of the jet is
distributed among the different particles in the spray.
Normal jets produced by quarks and gluons typically have the majority of their energy aligned with a single
central axis.
But jets resulting from heavier particles can have their energy spead along
2,3, or more axes depending on the type of heavy particle. 
There is a huge space of possibilities for how the substructure of jets from new particles could
look like.
A program of searches targetting different substructure signatures has been
ongoing for the last 10 or so years, but they have mostly focused on the
simplest cases, leaving large number of possibilities still open.


This search casts a wide net for these type of signatures, looking for any new
particle decaying to two jets with 'anomalous' substructure, distinct from
regular jets.
Because we didn't know exactly what we are looking for, we employed five different
anomaly detection methods to identify anomalous jets.
Each was done as essentially a separate analysis of the data, but all lived
within a common framework: each method started with the same set of events,
selected the events that appeared anomalous, and then used a common
statistical framework to analyze the results.  
The most important step is of course the selection of the anomalous events 
and because each method made different assumptions and used different techniques, we
expected them to have complementary sensitivity to different types of
anomalies.


<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/CASE_AI_algos.png' | relative_url }}" alt="" title="Different types of anomaly detection algorithms used" width="800"/>
    </div>
</div>
<div class="caption">
    We used several different types of algorithms to identify potential anomalies.
</div>


The different algorithms were based on different philosophies of how to
identify anomalous jets.
One approach, based on 'unsupervised' learning, trained a model that
learned what standard jets looked like and then selected events that looked
dissimilar from those as anomalous.
Three methods employed what is known as weak supervision, in which a model is
trained to separate two groups of jets in data, one of which is potentially
signal-enhanced.
The three methods were made different assumptions about the
signal and background, and therefore constructed the two groups of data jets differently. 
Tag N' Train was one of these methods, with its unique assumption being that
both of the jets in the event are anomalous (other methods assume it could
possibly be just one).
The last method was 'semi-supervised', a hybrid between a fully model agnostic and
standard approaches. This method used an ensemble of example signals as a 'prior' that potential anomalies may look like and 
then selected events that looked similar to these priors and dissimilar to
standard backgrounds. 

Before unleashing these methods on the real data, we did extensive testing in
simulation to validate them.
We verified that no method had a significant bias in its selection of anomalous
events such that the following statistical analysis would falsely claim
a signal (ie no false positives).
We also verified that for realistic amounts of signal present in the data,
these anomaly detection methods could greatly enhance the discovery potential
as compared to standard approaches. 
In many cases, a standard approach would have seen only very slight hints of
a new particle ($$\sim 2 \sigma$$) while multiple anomaly detection methods would
have been able to claim a discovery ($$> 5 \sigma$$)!
Our testing also showed that there was no one 'optimal' anomaly detection
method; they each performed better or worse on different signals. 

Unfortunately when applied to the actual data, no method found any significant
evidence of new particles :(

<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/CASE_vae_mjj.png' | relative_url }}" alt="" title="Comparison of sensitivity of different analysis methods" width="600"/>
    </div>
</div>
<div class="caption">
Mass distribution of the events that one AI algorithm selected as most anomalous. A new particle would have appeared as a ‘bump’, or excess of events at a certain mass value, on top of a smooth distribution of background events. Example bumps from two different signals are shown as blue and purple dashed lines. No such bump was observed.
</div>

The typical thing to do with a null result search is to place limits constraining the existence
of particles which your analysis was sensitive to.
While this is a standard practice for a null-result search, it required
significantly more work than usual for this type of analysis. 
First of all, I had to develop an entirely new method for calibrating the
modeling of the substructure of exotic jets (preliminary description is
[here](https://cds.cern.ch/record/2866330), paper will come soon).
The other vary serious complication is the performance of the weakly supervised methods
because the method performance (aka signal efficiency) changes dramatically depending on the amount of
signal in the data.
Running on data with no signal present tells you nothing about what you would
have seen had there been signal present (which is what you care about when
computing an exclusion limit). 
So we had to employ a novel statistical procedure to account for this, 
which meant a lot of scrutiny in CMS internal review before eventually being
ok'ed. 

In the end based on our results we were able to place limits constraining the existence of a wide variety of different particles.
Compared to a traditional search strategy optimized for a single particle's signature, 
the anomaly detection methods produced less constraining limits.
However, they were able to set limits on a much larger variety of signal models
than the traditional method (which is usually only be sensitive to that single particle).

After spending many months agonizing about limits, I came to prefer a different
method of quantifying the sensitivity of the anomaly detection methods. 
This metric computed how many signal events (aka cross section) would be needed to claim
a discovery ($$5 \sigma$$) from each method. 
We showed that for several signals the anomaly detection methods were able to claim a discovery
with a factor of ~3-4 fewer signal events
(Tag N' Train even reached up to a factor of 7 for one signal). 
I think this metric highlights the spirit of anomaly detection: Its about
increasing our chances of discovery! Not setting limits. 

This analysis was the first ever use of anomaly detection by CMS, and while it
isn't the first in particle physics overall (ATLAS has had a few results), its the best yet. 
We developed a lot of new techniques and know-how which should make future anomaly detection
searches a bit easier. 
This project isn't quite finished yet, stay tuned for future papers detailing the jet substructure calibration, ML details of the methods we used
as well as the final version of this physics results paper. 
But once all of that is wrapped up, I am excited to build on we developed here
for new anomaly detection searches in CMS!


<div class="row justify-content-sm-center">
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/CASE_limits.png' | relative_url }}" alt="" title="Limits" width="500"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/CASE_signif.png' | relative_url }}" alt="" title="Significance comparison" width="500"/>
    </div>
</div>
<div class="caption">
    Left, exclusion limits on the cross section of different new particles from the anomaly
    detection methods (various colors) as compared to several standard
    approaches (black, brown, tan, gray).
    Right, a comparison of the cross section needed for evidence (3-sigma) or discovery (5-sigma) for the
    different methods. 
</div>


To read more you can check out the CMS 'Physics Briefing' I wrote [here](https://cms.cern/news/can-ai-find-new-particles-its-own)
and the full paper
[here](https://cms-results.web.cern.ch/cms-results/public-results/preliminary-results/EXO-22-026/index.html). 

