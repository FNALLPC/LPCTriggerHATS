---
title: "Finding information about a trigger path"
teaching: 10
exercises: 10
objectives:
- "Learn different ways to look up information about a trigger path"
---

> ## Prerequisites
> Set up your machine following instructions in [setup][lesson-setup] first.
{: .prereq}

### Find the L1 seed of the MET HLT path
There are different ways to learn which is the L1 seed of a specific HLT path. Two examples will be tested in this exercise:
 * looking into [OMS](https://cmsoms.cern.ch/);
 * looking into [web-based confdb](https://hlt-config-editor-confdbv3.app.cern.ch/);
 * inspecting a HLT configuration.

In the context of this exercise, we will retrieve the information of the `HLT_PFMET170_HBHECleaned_v*` path from OMS for a specific run (`run 284043`) and HLT configuration (`/cdaq/physics/Run2016/25ns15e33/v4.2.3/HLT/V2`).

### OMS method
As a first step, connect to [OMS](https://cmsoms.cern.ch/cms/runs/report?cms_run=284043&cms_run_sequence=GLOBAL-RUN) 
 * Click on `"Runs > Triggers > HLT Path Report"`, 
 * Use search option at the bottom left ("FILTER > Path Name") to look for `HLT_PFMET170_HBHECleaned_v*`.
 * Find the information about the L1 seed names of the HLT path of interest. 

Then move to the L1 Trigger rate page in OMS and search for the L1 seeds there to have a look at the rates and prescales of these L1 seeds.<br>
(Note that you might need to increase the number of rows at the bottom of the page to find them.) 
### Web-based confdb
The [web-based confdb](https://hlt-config-editor-confdbv3.app.cern.ch/) is a web-based GUI of the database that holds all of CMS HLT configurations.<br>
One can **inspect** any given HLT menu without downloading one or connecting to the database.<br>
More information about the web-based confdb can be found in these [slides](https://indico.cern.ch/event/1230321/contributions/5177149/attachments/2580137/4450016/webbased%20confdb.pdf).

Let's try to find the `/cdaq/physics/Run2016/25ns15e33/v4.2.3/HLT/V2` menu in the GUI.
 * Connect to [https://hlt-config-editor-confdbv3.app.cern.ch/](https://hlt-config-editor-confdbv3.app.cern.ch/)
 * Click "Configurations > Open remote"
 * In the drop-down menu, select `online`
 * Follow the directory of the menu `/cdaq/physics/Run2016/25ns15e33/v4.2.3/`
 * Select "V2" from the side penal, and click "OK"

After retriving from the DB, you can see all the paths. 
 * Type the name of the path `HLT_PFMET170_HBHECleaned` into the search box in the left panel
 * Expand the path. This shows all the **modules** that the HLT path will run in sequence.
 * The L1 seed of a HLT path is always the first module (after the `HLTBeginSequence`) starting with the name `hltL1sXXXX`
 * Click on `hltL1sETM50ToETM120`
 * In the right panel, the `L1SeedsLogicalExpression` shows the L1 seeds used by the path.
> ## Checklist
> You should see the same L1 seeds that you find from OMS.
{: .checklist}

### Inspecting a HLT configuration
`hltGetConfiguration` is the official command to retrive a HLT configuration from the database.<br>
> ## Caution
> Since `hltGetConfiguration` involves connecting to the confdb database directly, this command should never be used in a large number of jobs or programatic loop. **It should only be used interactively.**
{: .caution}
~~~
ssh -f -N -D 1080 <yourUserName>@lxplus.cern.ch 
hltConfigFromDB --configName --adg /cdaq/physics/Run2016/25ns15e33/v4.2.3/HLT/V2 --dbproxy --dbproxyhost localhost --dbproxyport 1080 > dump_hlt_online_2016G.py
~~~
If you have connection issue, simply inspect one that has been downloaded for you
~~~
xrdcp root://cmseos.fnal.gov//store/user/cmsdas/2023/short_exercises/Trigger/dump_hlt_online_2016G.py .
~~~
Then, inspect the HLT configuration `dump_hlt_online_2016G.py` to look for the information about the L1 seed of the path into it:
~~~
grep 'process.HLT_PFMET170_HBHECleaned_v9' dump_hlt_online_2016G.py
~~~
You should see the expected output below:
~~~
process.HLT_PFMET170_HBHECleaned_v9 = cms.Path(process.HLTBeginSequence+process.hltL1sETM50ToETM120+process.hltPrePFMET170HBHECleaned+process.HLTRecoMETSequence+process.hltMET90+process.HLTHBHENoiseCleanerSequence+process.hltMetClean+process.hltMETClean80+process.HLTAK4PFJetsSequence+process.hltPFMETProducer+process.hltPFMET170+process.HLTEndSequence)
~~~
> ### Questions
> To conclude, answer the following questions:
>  * Which was the lowest threshold L1 seed active in the L1 menu?
>  * Which is the lowest threshold L1 seed unprescaled?
>  * How is it called the HLT module that contains the information about the L1 seeding?
{: .challenge}

