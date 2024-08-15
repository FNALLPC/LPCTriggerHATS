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
> ### Extract the MET turn-on of the MET HLT path
> Glimpse through the configuration file in `test/ana_METMiniAOD.py` and try to get a general idea of what it does.<br>
> [Lines 45-48](https://github.com/FNALLPC/LPCTriggerHATS/blob/2024/ShortExerciseTrigger/test/ana_METMiniAOD.py#L45-L48) of this configuration file show that it invokes an analyzer module called `METTrigAnalyzerMiniAOD`, and gives it two HLT paths as input parameters (with a specific version number that keeps track of minor updates to the HLT path from one menu to another one):
> ~~~
> process.metTrigAnalyzerMiniAOD = cms.EDAnalyzer("METTrigAnalyzerMiniAOD")
> process.metTrigAnalyzerMiniAOD.refTriggerName = cms.untracked.string("HLT_Ele27_eta2p1_WPTight_Gsf_v7")
> process.metTrigAnalyzerMiniAOD.sigTriggerName = cms.untracked.string("HLT_PFMET170_HBHECleaned_v6")
> ~~~
> We will use the `METTrigAnalyzerMiniAOD` analyzer module to perform a simple trigger efficiency measurement.<br>
> We will use `HLT_Ele27_eta2p1_WPTight_Gsf` as a reference trigger to measure the trigger efficiency of the `HLT_PFMET170_HBHECleaned` signal trigger.
> 
> Next, check through the code in `plugins/METTrigAnalyzerMiniAOD.cc` and try to get a general idea of what it does.
> 
> Then run the configuration file as follows:
> ~~~
> cd test
> voms-proxy-init --voms cms
> cmsRun ana_METMiniAOD.py 
> ~~~
> Use the TBrowser to explore the file `histos_METTrigAnalyzer.root` and look at the histograms `h_met_all` and `h_met_passtrig`. 
> > ## Question
> > Can you explain the shape of each distribution?
> {: .challenge}
> 
> The plotting macro `plot_trigeff_met.C` uses the `TEfficiency` class to create an efficiency plot using the two histograms produced above.
> 
> Inspect the code in `plot_trigeff_met.C` and then run this macro as follows:
> ~~~
> root -l plot_trigeff_met.C
> ~~~
> The resulting turn-on is the result of our efficiency measurement.<br>
> It shows the trigger efficiency of the signal trigger (vertical axis) in bins of offline-reconstructed MET (horizontal axis).
> > ## Question
> > Inspect the resulting efficiency plot. At which value of offline MET does the trigger turn-on reach its maximal value, and the flat "plateau" region start?
> {: .challenge}
{: .solution}

> ## NanoAOD
> A centrally maintained NanoAOD format was proposed in 2018, aiming for a common Ntuple format that can be used by most of the CMS analysis groups. 
> Information about the NanoAOD format can be found [here](https://cms-nanoaod-integration.web.cern.ch/integration/master-102X/mc102X_doc.html#HLT). 
> 
> In this exercise, we will repeat the trigger efficiency measurement using a NanoAOD input file (events stored in it are different from the MiniAOD file used earlier) format and the related tools.
> 
> First, check out the NanoAOD-tools package for reading NanoAOD files:
> ~~~
> cd $CMSSW_BASE/src
> git clone https://github.com/cms-nanoAOD/nanoAOD-tools.git PhysicsTools/NanoAODTools
> cd PhysicsTools/NanoAODTools
> scram b -j 4
> cd $CMSSW_BASE/src/ShortExerciseTrigger2023/ShortExerciseTrigger/test
> ~~~
> {: .language-bash}
> Copy the skimmed NanoAOD file to your working directory as follows:
> ~~~
> xrdcp root://cmseos.fnal.gov//store/user/cmsdas/2023/short_exercises/Trigger/New_NanoAOD_M1000.root .
> ~~~
> Then, first run the code (written using packages of NanoAOD tools available centrally) to produce the required histograms to compute the efficiency.<br>
> Next run the code to take the ratio of the two histograms and plot the efficiency and store it as a pdf:
> ~~~
> python MET_Efficiency_NanoAOD_gp.py
> python MET_Efficiency_Plotting.py
> ~~~
> > ## Question
> > Inspect the resulting efficiency plot. Though we used a different sample, is the shape of this result consistent with what you obtained in MiniAOD?
> {: .challenge}
{: .solution}

{% include links.md %}

