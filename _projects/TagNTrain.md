---
layout: page
title: Tag N' Train
description: A new method for anomaly detection at the LHC
img: /assets/img/TNT_cwola_wide.png
importance: 1
category: work
giscus_comments: false
related_publications: true
---
![image](/assets/img/TNT_cwola.jpg){: width="200" style="float: right"}



One of the main goals of LHC experiments is to look for signals of physics beyond the Standard Model; 
new particles that may explain some of the mysteries the Standard Model doesn’t answer. 
The typical way this works is that theorists come up with a new particle that would solve some mystery and they spell out how it interacts with the particles we already know about. 
Then experimentalists design a strategy of how to search for evidence of that particle in the mountains of data that the LHC produces. 
Designing such searches is a labor intensive process, involving simulating the
hypothetical signal, designing a strategy to select potential signal events out
of the large backgrounds (often using a Machine Learning based classifier), and
then designing a strategy to estimate the amount of background that will remain
after selection.
So far none of the searches performed in this way have seen any definitive evidence of new particles, but there is still a lot of unexplored territory.
Because of limited person-power there will always be signatures we are unable
to look for. 


<div class="row justify-content-sm-center">
    <div style="text-align: center">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/theories_of_dark_matter.png' | relative_url }}" alt="" title="Dark matter theories" width="800"/>
    </div>
</div>
<div class="caption">
    There are *a lot* of ideas out there for new particles... (Image stolen from Tim Tait)
</div>

But what if instead of looking for new particles one-by-one, we could use the
data itself tell us what to look for? 
The basic idea is that you can use part of the data itself to train a neural network to find a signal, 
and then use the rest of the data to actually look for that signal. 
This lets you search for many different kinds of models at the same time!


The hard part then becomes: "How can I train a neural network on the real data given that
I don't know what the signal events in the data are?"
The technique we came up with is based on the idea of 'Classification Without
Labels' (abbreviated CWoLa, pronounced like koala). This is a 'weak
supervision' technique where instead of needing the true labels of your
training set you only need to be able to split your data into two groups, one
of which has more signal in it than the other. 
The question is then "How can I split my data into these groups?", and thats
where the Tag N' Train algorithm comes in.
The basic idea is that many of the particles we are looking for have
a signature in which they produce *pairs* of anomalous objects (in our case,
jets). 
For every event then you can *tag* an event as signal-like or background-like using one of the
objects, and use that to *train* a classifier for the other object. 
The initial classification can be based on a classifier trained in an unsupervised
method, like an autoencoder or any *a priori* information you have about the
signal. 

<div class="row justify-content-sm-center">
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/TagNTrain_diagram.jpg' | relative_url }}" alt="" title="Tag N' Train"/>
    </div>
    <div class="col-sm mt-4 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/TNT_roc_event_1p.png' | relative_url }}" alt="" title="Classification Performance"/>
    </div>
</div>
<div class="caption">
    Left, a flow-chart for the Tag N' Train algorithm. Right, a ROC curve
    showing the classification performance of the TNT classifier as compared to
    some baselines. See the paper
    for more details.
</div>


In our testing, Tag N' Train classifier can greatly improve upon the initial
unsupervised classifier, compares favorably with other state of the art
techniques for anomaly detection at the LHC. 
Whats more, Tag N' Train was put to the test in the 'LHC Olympics',
a competition which released a mock collider dataset containing an unknown hidden
signal.
Tag N' Train was one of the few entries able to correctly find the hidden
signal!


Tag N' Train was used as part of the [CMS anomaly detection search](projects/CASE) that
I led and performed very well!

To read more about the method can check out our paper on Tag N' Train paper {% cite TNT %}
and our results in the LHC olympics paper {% cite LHCO %} . 

