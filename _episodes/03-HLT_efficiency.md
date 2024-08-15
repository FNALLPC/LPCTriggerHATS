---
title: "Measuring trigger efficiencies"
teaching: 30
exercises: 0
objectives:
- "Measure trigger efficiency using the tag-and-probe method"
---

> ## Prerequisites
> Set up your machine following instructions in [setup][lesson-setup] first.
{: .prereq}

> ## Objective
> The goal of the following exercises is to learn how to access and play with the trigger objects in our data and compute the efficiency of a specific HLT path and look also at its Level 1 (L1) seed. 
>
> The focus will be on a HLT path used during the 2016 data-taking to select events with a certain amount of missing transverse energy: `HLT_PFMET170_HBHECleaned_v*`.
{: .objectives}

## Ex 1: Access trigger bits and compute a MET trigger efficiency

In exercise 1 we performed a simple trigger efficiency measurement for a MET trigger (signal trigger) with the help of a second, unbiased trigger (reference trigger). 
Now we will present another common method: the tag-and-probe method, which we use to estimate the trigger efficiency of our single-muon trigger HLT\_IsoMu24.

The tag-and-probe method is based on the fact that the properties of two objects, in our case the two muons, are independent of each other. Therefore the probability 
that one muon causes the trigger to fire does not depend on the probability that the other muon makes the trigger fire.

The "tag" muon is required to be matched to a trigger-level muon object. This means that presence of this tag muon in the event alone is enough to fire the trigger. Then 
we search for another offline muon, a "probe", and ask: if the tag muon would not have been there, could this "probe" muon have fired the trigger anyway?

In the code, this is done in lines 277-283 of plugins/SingleMuTrigAnalyzerMiniAOD.cc, where we check if the probe muon is matched to a trigger-level muon object:
> ## MiniAOD
> The MINIAOD format was introduced at the beginning of Run 2 to reduce the information and file size from the AOD file format. 
> For Run 2 analyses, most of the analysis groups at CMS skimmed the centrally produced MiniAOD files into smaller, analysis-specific ROOT Ntuples. 
> This means that several redundant versions of Ntuples for different analysis groups are stored in the limited CMS storage spaces.
{: .solution}

> ## NanoAOD
> A centrally maintained NanoAOD format was proposed in 2018, aiming for a common Ntuple format that can be used by most of the CMS analysis groups. 
> Information about the NanoAOD format can be found [here](https://cms-nanoaod-integration.web.cern.ch/integration/master-102X/mc102X_doc.html#HLT). 
{: .solution}

{% include links.md %}

