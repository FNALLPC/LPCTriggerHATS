---
title: "Measuring trigger efficiencies"
teaching: 30
exercises: 0
objectives:
- "Learn how to access the trigger information stored in MiniAOD and NanoAOD"
- "Learn what is trigger objects and how to access them"
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

We will first run this exercise on MiniAOD format, then run it again on NanoAOD.

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

