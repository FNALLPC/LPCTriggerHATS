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

## Compute a MET trigger efficiency

We will first run this exercise on MiniAOD format, then run it again on NanoAOD.

> ## MiniAOD
> The MINIAOD format was introduced at the beginning of Run 2 to reduce the information and file size from the AOD file format.<br>
> This means that several redundant versions of Ntuples for different analysis groups are stored in the limited CMS storage spaces.<br>
> For Run 2 analyses, most of the analysis groups at CMS skimmed the centrally produced MiniAOD files into smaller, analysis-specific ROOT Ntuples.<br>
> 
> MiniAOD events contain two trigger products that we will need in these exercises.<br>
> The `TriggerResults` product contains trigger bits for each HLT path, whereas the `TriggerObjectStandAlone` product contains the trigger objects used at HLT.<br>
> In addition, the trigger prescales, L1 trigger decisions, and L1 objects are stored in MiniAOD.<br>
> A more detailed description of the trigger-related MiniAOD event content can be found [here](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookMiniAOD2016#Trigger).
> 
> In this exercise we work with a skimmed MiniAOD file. (In case you are wondering where this skimmed file came from: it has been created using the configuration in `ShortExerciseTrigger/test/skim_pfmet100.py`, which selects events with offline MET above a threshold of 100 GeV.)
> 
> > ## Inspect MiniAOD content
> > First, inspect the contents of the skimmed MiniAOD input file as follows:
> > ~~~
> > edmDumpEventContent root://cmseos.fnal.gov//store/user/cmsdas/2023/short_exercises/Trigger/skim_pfmet100_SingleElectron_2016G_ReReco_87k.root --regex=Trigger
> > ~~~
> > You can also inspect the full file content by dropping the --regex parameter.
> {: .challenge}
> 
> As you see, there are indeed multiple TriggerResults products here, as well as other trigger-related collections.<br>
> We will learn how to interact with these two products and how to use their packed information in our physics analyses in this and the following exercises.
> 
{: .solution}

> ## NanoAOD
> A centrally maintained NanoAOD format was proposed in 2018, aiming for a common Ntuple format that can be used by most of the CMS analysis groups. 
> Information about the NanoAOD format can be found [here](https://cms-nanoaod-integration.web.cern.ch/integration/master-102X/mc102X_doc.html#HLT). 
{: .solution}

{% include links.md %}

