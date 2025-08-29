---
layout: page
title: CaloDiffusion
description: Diffusion models for LHC calorimeter simulations
img: /assets/img/calo_challenge.jpg
importance: 1
category: work
giscus_comments: false
related_publications: true
---

Simulations play a crucial role in making sense of the massive amounts of data
produced by the LHC.
When sifting through the extremely complex data produced by our detectors, we
need a handle on what were are looking at.
Being able to simulate what the collisions of known processes would like in our
detector, and what the signatures of hypothetical new particles would leave is
essential. 

One crucial aspects of these simulations is the detector response, 
We need to know what sort of signals we would see in our detector if a particle of a given type and
energy were passing through. 
Fortunately we have a very accurate first-principles understanding of how
particles interact with the materials of our detector, available in a public
software package GEANT. 
However, these simulations can be quite computationally expensive to perform, 
particularly for the 'calorimeter' portion of the detector.
In calorimeters, a single high energy particle will interact with the particles
present in the material to produce a 'shower' of
secondary particles (each of which can further produce additional showers,
etc), which must be accurately simulated to get a realistic estimate of the
detector signal. 
As we move towards higher data volumes in the High-Luminosity era of the LHC,
and develop even more complex calorimeters (like the CMS HGCAL), we will no
longer have the computing resources to perform these detailed calorimeter
simulations. 
This would significantly hurt the precision of our data analysis techniques 
unless a solution can be found.

Thats where generative machine learning may come to the rescue. 
If we can train a machine learning model that can 
accurately generate these calorimeter showers faster than GEANT, then it could
be used GEANT's place in the simulation workflow.
People in the HEP+ML community have been working on this problem for a few
years and a variety of approaches are being explored.
Our approach, **CaloDiffusion** {% cite CaloDiffusion %}, is based on a type of generative machine learning model called
'diffusion', which has extemely popular starting in ~2022.
It is the backbone behind most of the popular AI image generation technologies: DALLE, MidJourny, StableDiffusion, etc.
Using CaloDiffusion, we are able to obtain showers that are nearly
indistiguishable from Geant.


<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/avg_showers.png' | relative_url }}" alt="" title="Average Calorimeter Showers" width="600"/>
    </div>
</div>
<div class="caption">
A comparison of the average Calorimeter showers produced by GEANT versus
CaloDiffusion on a sample dataset based on photons hitting a 5 layer detector. 
The energy deposits in all five layers are shown, the energy deposition in each
layer has a circular symmetry. 
</div>


Diffusion works as a noising/denoising process. 
One starts with a normal image you can define a noising process that gradually
adds noise to it across many steps (around 100 steps is typical), until the
image is essentially pure noise.
The machine learning model is then trained to reverse this process;
by learning how to remove the noise you have added at each step.
Once trained, this model can then generate new images by starting with a pure
noise image and iteratively applying its denoising step.
Additional 'conditioning' information about the target image can also be given to the model, 
during training to help with the denoising and to allow one to guide the type
of image that is generated once trained. 
This is particularly important for our use case, since we don't want to
generate a random particle shower, but a shower that corresponds to the
specific particle type and energy that we want to simulate. 

<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/diffusion_diagram.jpg' | relative_url }}" alt="" title="Diffusion diagram" width="800"/>
    </div>
</div>
<div class="caption">
    A diagram showing the basics of the diffusion process. The $q(x_{t-1}| x_t)$ function
    iteratively adds noise to the image.
    The $p_{\theta}(x_t | x_{t-1})$ function is the model that learns to invert this noising
    process, and can generate new images starting from pure noise. 
</div>

Calorimeter data isn't exactly like images but they have some simularities.
Energy is deposited into different sensors across several layers of material.
The full shower is then similar to a 3D image, where each 'voxel encodes
the energy in a given cell.
Unlike objets in images which have an x and y translation symmetry,
showers typically have cylindrical symmetry when centered on the point of
impact of the initial particle.

For these reasons, in CaloDiffusion we replaced the use of regular convolutions
used in standard image processesing models with 3D, cylindrical convolutions,
which better encode the symmetries of our data. 
We also included a new method to allow these convolutions to be
conditional on the shower layer and radial position, which allows more
expressive freedom to the model without any additional parameters. 

Unfortunately, realistic detectors never have perfect cylindrical structure.
Different layers are often made of different materials, have different sensor
sizes and layouts. 
This makes the inherent data structure of these shows very irregular, which is
difficult to deal with for machine learnin models.
To handle this difficulty while retaing the geometric power of convolutions, 
we developed a new embedding approach called **GLaM** which performs a lightweight
mapping from the irregular geoemtry to a regular cylindrical shape, which can
then be processed with our cylindrical convolutions.
This embedding is learned simulataneously with the denoising model. 


<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/glam_diagram.png' | relative_url }}" alt="" title="GLaM diagram" width="800"/>
    </div>
</div>
<div class="caption">
A diagram showing the GLaM approach. A mapping from the irregular input shape
to the desired regular shape is learned. The denoising process based on
cylindrical convolutions is learned on the regular shape, and then 
outputted back to the original irregular shape.
</div>


We applied our approach to three different datasets, representing three
different detectors, that were part of the
community [Fast Calorimeter Simulation
Challenge](https://calochallenge.github.io/homepage/). 
The first dataset is based on part of the ATLAS detector and includes both
photon and pion showers. 
Because it is based on the real ATLAS detector it has realistic irregularities
between the different layers, which we handle with our GLaM approach. 
The total dimensionality of these showers is ~500 voxels.
The second and third datasets are based on an idealized perfectly cylindrical
detector, but feature much high dimensionality (6480 and 40,000 voxels
respectively).


We then compared the showers generated by CaloDiffusion and Geant across many
different features and found CaloDiffusion was able to match Geant extremely well across nearly all of them. 


<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/calodiffu_energy_comparisons.png' | relative_url }}" alt="" title="Diffusion diagram" width="800"/>
    </div>
</div>
<div class="caption">
Some quantitative comparisons of the showers produced by Geant and
CaloDiffusion on datasets two and three of the CaloChallenge. 
CaloDiffusion is able to match the distribution of energy as function of the
layer (left) and radial dimmension (center). The width of the shower in the
radial dimmension is also well modeled (right). 
</div>

We also perfomed quantitative comparisons between the two sets of showers.
One popular metric is to train a NN classifier to attempt to distinguish
between the two sets of showers.
If this classifier is unable to perform well (AUC $\to$ 0.5), this indicates
the two samples of showers are nearly indistinguishable.
We found classifier AUC's of 0.55--0.65 on the different datasets,
the best values reported to date. 
Our approach particularly shined on the highest dimensional dataset, in which 
we significantly outperformed previous approaches.


Further comparisons between CaloDiffusion and other approaches were perfomed 
as part of the CaloChallenge {% cite CaloChallenge %}. 
Out of approximately 40 total submissions CaloDiffusion was in the top 2 of shower quality for all datasets
(the other high quality submission, also based on diffusion, was released
a year after CaloDiffusion). 

Further improvements to CaloDiffusion are in progress, and we hope to apply the
model to simulate the CMS HGCAL in the near future!

