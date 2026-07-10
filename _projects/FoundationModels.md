---
layout: page
title: Foundation Models for Physics
description: Pre-training on real LHC data, and how physics data scales
img: /assets/img/foundation_aoj_detector.png
importance: 1
category: Foundation Models
giscus_comments: false
related_publications: true
---

Some of the most dramatic advances in AI over the last few years have come from
*foundation models*: enormous neural networks pre-trained on huge piles of data,
which can then be adapted ("fine-tuned") to a whole range of downstream tasks.
The large language models behind ChatGPT are the famous example --- a single
model is trained on a big chunk of the internet, and that same model can then be
specialized to write code, answer questions, or translate languages. The magic
is that the model learns a general-purpose *representation* of its data during
pre-training, so the downstream tasks get a huge head start instead of having to
learn everything from scratch.

Particle physics would seem like an ideal place for this idea. The LHC produces
staggering amounts of complex data, and we constantly build specialized models
for individual tasks --- tagging jets, generating simulated events, separating
signal from background. If we could pre-train one big model that understands the
general structure of collider data, every one of those downstream tasks could
benefit. But bringing foundation models to physics raises two basic questions
that I've worked on with my collaborators:

1. **Where does the pre-training data come from?** Foundation models are hungry
   for data, and physicists usually train on *simulation*, which takes computational resources to produce
   and is never a perfect match to reality.
2. **Do the "scaling laws" that make foundation models work even hold for
   physics data?** In language, model performance improves in a remarkably
   predictable way as you add more parameters, data, and compute. Is the same
   true for collider physics?

### Unlocking real LHC data: Aspen Open Jets

The first question is really about data. It turns out the LHC experiments have
already released large amounts of real collision data to the public --- but it's
stored in complicated experiment-specific formats that are very hard for anyone
outside the collaboration to use. This data has been largely untapped for
machine learning.

In the first project {% cite AOJ %}, my collaborators and I processed a huge
sample of CMS open data into a clean, machine-learning-friendly format we call
**Aspen Open Jets (AOJ)**. It contains roughly **180 million jets** --- sprays
of particles from the LHC's most common collisions --- making it by far the
largest dataset of its kind, and crucially it is built from *real* data rather
than simulation. We released it publicly so the whole community can use it.
This is only a small fraction of the data collected by CMS, so this could be scaled up
quite a bit further if CMS decided to release more data publicly. 

One nice thing about this dataset is that you can literally *see* the
CMS detector in the raw data. If you plot where the particles land in the
detector, the grid-like segmentation of the calorimeter cells, the boundaries
between detector regions, and even individual dead readout channels are all visible.

<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/foundation_aoj_detector.png' | relative_url }}" alt="" title="Particle positions in the CMS detector" width="650"/>
    </div>
</div>
<div class="caption">
    The positions of particles in the Aspen Open Jets dataset, in the
    eta-phi plane (roughly, the angle along the beam versus around it).
    The grid-like texture reflects the granularity of the real CMS detector cells,
    the vertical bands mark the transition between detector regions, and the
    scattered dark spots are individual dead cells. 
</div>

To show this data is actually *useful* for foundation models, we pre-trained one
on it. We used a model called OmniJet-α, which treats a jet much like a language
model treats a sentence: each particle is converted into a "token," and the model
learns to predict the next token in the sequence. After pre-training on the 180M
real jets, we fine-tuned the model on a much harder, deliberately mismatched
task --- generating *simulated* jets of a different type (top-quark jets) with
different kinematics. This is a big "domain shift", and therefore an interesting test of
the value of pre-training.

The pre-training paid off. The pre-trained model reached high-quality generation with far less
fine-tuning data than a model trained from scratch --- often about **10 to 100
times fewer** jets to reach the same quality. Even though it was pre-trained on
real QCD jets and asked to generate simulated top jets, it smoothly adapted,
essentially "morphing" what it already knew about jets to fit the new task. A
model starting from scratch had to relearn everything and needed far more data
to catch up.

<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/foundation_transfer_learning.png' | relative_url }}" alt="" title="Transfer learning results" width="650"/>
    </div>
</div>
<div class="caption">
    Generating top-quark jets after fine-tuning on different amounts of data
    (colored lines, ranging from 100 up to 10 million training jets). The left
    column shows the pre-trained model (fine-tuned on AOJ) and the right column a
    model trained from scratch, each compared against the target JetClass
    distribution (grey), across three physical features of the jets (their
    momentum, mass, and substructure). With little data, the pre-trained
    model already produces jets close to the target, while the from-scratch model
    is far off --- the two only converge once a large amount of data is available.
</div>

### Do physics models scale like language models?

The second question is more fundamental. The reason people pour billions of
dollars into ever-larger language models is the discovery of *neural scaling
laws*: as you increase the model size, the amount of training data, and the
compute, performance improves in a smooth, predictable, power-law fashion. These
laws are what tell you it's worth building a bigger model --- you know roughly
what you'll get. But there was no guarantee that collider data, which is
very different from human language, would behave the same
way.

In a follow-up project {% cite Jet_scaling %}, mostly led by collaborators, we
set out to measure the scaling
laws for jet generation for the first time. The results were a mix of reassuring
and surprising. When we scaled up the *model size*, we did recover the clean
logarithmic scaling behavior familiar from language models: bigger models
reliably generate better jets. But when we scaled up the *dataset size* and
*compute*, the improvement was much weaker than in language: the models saturate
quickly, hitting a plateau where extra data barely helps.

<div class="row justify-content-sm-center align-items-center">
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/foundation_scaling_kaplan.png' | relative_url }}" alt="" title="Neural scaling law for language models (Kaplan et al.)" width="450"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/foundation_scaling_jets.png' | relative_url }}" alt="" title="Scaling of jet generation with dataset size" width="450"/>
    </div>
</div>
<div class="caption">
    Left: the famous neural scaling law for language models (the dataset-size
    panel from <a href="https://arxiv.org/abs/2001.08361">Kaplan et al.</a>),
    where the loss keeps falling as a clean straight power law as you feed in more
    data --- the trend that justifies building ever-larger models. Right: our
    measurement of the same thing for jet generation. Instead of a straight line,
    the loss quickly flattens out and hits a floor (dotted line), where adding
    more data barely helps.
</div>

To make sense of this, we introduced the idea of a **"learnable window."**
Intuitively, a jet is far more random than a sentence: it's the product of the
strong nuclear force spraying out particles in an intrinsically stochastic way,
so beyond a certain point there simply isn't more structure left to learn from
additional examples. The model quickly absorbs the learnable structure and then
plateaus, because the rest is irreducible randomness. 
This appears to be a real difference
between generative modeling of physics data and next-word prediction in text,
and it has serious implications for how we might approach building a foundation model in physics. 

If the saturation we observed is real, we need to rethink how we might train a physics foundation model.
Copying the next-token-prediction task from the language domain, which many assumed would work, may be a dead end.
So either we need a different unsupervised training objective, which actually scales as we would hope and can be deployed on the unlabeled data collected by our experiments.
Or we use supervised pre-training objectives, which [have been shown to exhibit nice scaling behavior](https://atlas.web.cern.ch/Atlas/GROUPS/PHYSICS/PUBNOTES/ATL-SOFT-PUB-2026-002/), but this may suffer from the limited quality of our simulation, and bias us toward the known physics models we choose to simulate.

### Where this is going

Taken together, these projects chip away at two of the practical barriers to
building foundation models for physics: they provide a large, real-world dataset
to pre-train on, and they map out how much we should actually expect to gain from
scaling. 
Foundation models are still in their early days in our field.
Scaling laws have clearly proved extremely powerful in language, and we should
figure out how they can be harnessed for particle physics.
I would also like to understand what qualitatively new capabilities a successful foundation model could 
achieve. 

You can read more in the Aspen Open Jets paper {% cite AOJ %} and the scaling
laws paper {% cite Jet_scaling %}. The AOJ dataset is public for anyone to use.
