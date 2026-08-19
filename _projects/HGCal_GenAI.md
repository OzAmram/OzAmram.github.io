---
layout: page
title: Generative AI for HGCal
description: Benchmarking generative models as fast simulation for the CMS calorimeter upgrade
img: /assets/img/HGCal_detector.png
importance: 3
category: Applications of Generative Models
giscus_comments: false
related_publications: true
---

In [CaloDiffusion](/projects/CaloDiffusion.html) we showed that diffusion
models can generate calorimeter showers that are nearly indistinguishable from
Geant. But that was demonstrated on the
[CaloChallenge](https://calochallenge.github.io/homepage/) datasets, which are
either idealized cylindrical detectors or a simplified slice of ATLAS.

The goal throughout has been to get these models running inside a real
experiment. This project is that step: building generative fast simulation for
the CMS High-Granularity Calorimeter (HGCal), with deployment in the CMS simulation
workflow as the target.

### Why HGCal is hard

HGCal is the upgrade that will replace the CMS endcap calorimeters for the
High-Luminosity LHC. It is a very complex detector --- 47 layers, roughly
six million silicon channels, and sensors laid out on an irregular hexagonal
grid whose cell sizes and materials change as you move through the different layers.

<div class="row justify-content-sm-center align-items-center">
    <div class="col-sm-5 mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_detector.png' | relative_url }}" alt="" title="Cross section of the CMS High-Granularity Calorimeter" width="400"/>
    </div>
    <div class="col-sm-7 mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_hex_cells.png' | relative_url }}" alt="" title="Hexagonal sensor cell layout on an HGCal silicon wafer" width="600"/>
    </div>
</div>
<div class="caption">
    Left: a cross section of HGCal. The electromagnetic section (CE-E) is 26
    layers of silicon interleaved with lead and copper-tungsten; the hadronic
    section (CE-H) adds 21 more layers of silicon and scintillator in steel and
    copper. Right: the sensor layout on an 8-inch silicon wafer, with the larger
    1.18 cm<sup>2</sup> cells (left) and the smaller 0.52 cm<sup>2</sup> cells
    (right) used in different regions of the detector colours mark the cells. 
    Between the hexagonal tiling and the change in cell size across the detector, the data has none of the regular
    structure that traditional ML architectures rely on. Figures from the
    <a href="https://cds.cern.ch/record/2293646" target="_blank" rel="external nofollow noopener">HGCal Technical Design Report</a>.
</div>

This complexity also makes simulation expensive. Geant4's cost scales with
the detail of the geometry, and HGCal is far more detailed than any calorimeter before it. 
Meanwhile the simulation share of the CMS computing budget is
shrinking, not growing, as reconstruction gets more demanding.
So fast simulation will be a requirement in the High Luminosity era. 
Generative AI can ensure we don't lose physics performance because of it.

For a sense of the jump in scale: the showers here have roughly 500,000
active cells, about **ten times** the highest-granularity dataset in the
CaloChallenge. That is after restricting to a per-layer region of interest
around the shower axis: 25 cm for photon showers, growing from 42 to 127 cm with
depth for pion showers.

Raw cell count is not the only challenge; the irregular geometry is also
quite difficult to handle. 
The cells are hexagonal, their sizes vary across the detector,
and the layout has none of the regularity that convolutional models are built
to exploit. Irregular geometries were tested in the CaloChallenge, but only at much smaller scales of around 500 cells.

### A common CMS dataset

Rather than have everyone build their own setup and report incomparable
numbers, we put together a common CMS dataset and a shared evaluation framework.
The dataset is single particles fired into HGCal in CMSSW: photons and
charged pions, one million showers each, split 700k for training and 300k for
evaluation, at incident energies from 1 to 1000 GeV sampled log-uniformly.

The evaluation follows the CaloChallenge template. We compute a few hundred
physically meaningful features per shower --- per-layer energy fractions,
centroids, widths, cell occupancy, the transverse profile in rings of
hexagonal neighbours --- and then compare the generated and Geant distributions
using 1D separation power, unbinned Kolmogorov-Smirnov tests, the multivariate
FPD and KPD scores, and the AUC of a classifier trained to tell real showers
from generated ones.


We plan to release this dataset and evaluation code publicly in the future!

### Five models

We trained and evaluated five approaches, deliberately spanning the different
generative paradigms in play right now:

- **HGCaloDiffusion** {% cite CaloDiffusion %} --- an improved version of our CaloDiffusion model.
 A diffusion U-Net on a
  voxelized representation, carrying over the cylindrical convolutions and GLaM
  geometry embedding from CaloDiffusion but with a new model for the layer energies and better training/sampling methods.
- **HGCaloTrilogy** {% cite CaloTrilogy %} --- a next-generation model on top of
  HGCaloDiffusion, sharing its preprocessing and geometry handling but
  replacing the generative core with mean-velocity flow transport, a learned
  Gaussian mixture prior, and a physics-guided loss on high-level observables.
  This is the line of work that upgrades CaloDiffusion to handle the complexities
  of HGCal.
- **HGCaloDream** --- flow matching with a vision-transformer backbone, in a
  two-stage factorization.
- **AllShowers** --- a point cloud representation with conditional flow
  matching, and a single model handling both particle types.
- **GraphCNF** --- continuous normalizing flows on graphs, chaining a total
  energy flow, a per-layer graph flow, and a Riemannian flow-matching step for
  the individual cell energies. Photons only so far.

### So how did they do?

Pretty good! Most of the important features of this challenging dataset are well described by multiple models.

The marginal distributions are in good shape. Total energy response, shower
centroids and widths, and the shape as a function of depth are all reproduced
closely by most of the models.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_long_profile_photons.png' | relative_url }}" alt="" title="Longitudinal shower profile, photons" width="500"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_long_profile_pions.png' | relative_url }}" alt="" title="Longitudinal shower profile, pions" width="500"/>
    </div>
</div>
<div class="caption">
    Average fraction of the shower energy landing in each of the 47 layers, for
    photons (left) and pions (right), with the models overlaid on Geant4 in
    black and the ratio to Geant4 underneath. The pion profile shows the jumps
    where the detector changes character between the electromagnetic and
    hadronic sections. Most models sit within a few percent of Geant across the
    bulk of the shower, with GraphCNF being an outlier on the photons.
</div>

Zooming in on a single layer tells the same story, and also shows where it
starts to break down. Layer 30 sits well inside the hadronic section, where
pion showers are at their broadest and messiest.

<div class="row justify-content-sm-center">
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_efrac_pions_layer30.png' | relative_url }}" alt="" title="Energy fraction, pions, layer 30" width="350"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_xcenter_pions_layer30.png' | relative_url }}" alt="" title="Shower centroid, pions, layer 30" width="350"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/HGCal_occupancy_pions.png' | relative_url }}" alt="" title="Cell occupancy, pions, layer 30" width="350"/>
    </div>
</div>
<div class="caption">
    Pion showers in layer 30: the fraction of the shower energy deposited in the
    layer (left), the transverse position of the shower centroid (middle), and
    the cell occupancy (right). The first two hold up across the bulk of the
    distribution, HGCaloDream being the exception on the centroid. 
    Occupancy was found to be the most challenging feature to capture and the agreement with Geant is visibly worse; no model captures the full tail behaviour.
</div>

The multivariate metrics, which are sensitive to correlations between features that the one-dimensional distributions miss, show good performance but with room for improvement.
No model yet reaches Geant-Geant indistinguishability.
HGCaloDream, the best model on the photon dataset, reports a classifier AUC of $$\sim$$ 0.56.
On the more challenging pion dataset HGCaloTrilogy and AllShowers are the best, at AUCs of $$\sim$$ 0.65.
These values are not far from full indistinguishability (AUC = 0.5).

There is also no single winner: the best model for pions is not the best for
photons, and within one particle type one model will lead on bulk shape while
another leads on width and occupancy. The architectures have complementary
strengths, and it is too early to consolidate around one.

### What's next

This is a first demonstration that generative models can actually handle the complexities of CMS HGCal. 
Further evaluation will be needed to assess the computational performance of the models and to validate them on
downstream physics observables.
However, these promising early results set us on track to deploy these models in the HL-LHC era, with a significant benefit to the 
physics program of CMS.


I am leading this effort within CMS, coordinating the teams behind the different
models along with the dataset and evaluation framework that lets us compare them
on equal terms.

Results so far are public as a CMS Detector Performance note,
[CMS-DP-2026-049](https://cds.cern.ch/record/2962239), with more detail on the
[public TWiki](https://twiki.cern.ch/twiki/bin/view/CMSPublic/PhysicsResultsDP2026049).

A full paper describing the dataset, the models, and the evaluation in detail is
in preparation!
CMS members can follow the analysis [here](https://cmsfence.cern.ch/alcm/cmsanalysis/details/ancode=MLG-26-001).
