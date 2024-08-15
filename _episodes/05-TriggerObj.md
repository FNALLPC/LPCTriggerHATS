---
title: "Accessing trigger objects"
teaching: 30
exercises: 0
objectives:
- "Learn how to access the trigger information stored in MiniAOD and NanoAOD"
- "Learn what is trigger objects and how to access them"
- "Measure trigger efficiency using the tag-and-probe method"
---

> ## MiniAOD
> In the [efficiency measurement Ex.][lesson-04-HLT_efficiency], we performed a simple efficiency measurement using the `TriggerResults` product in a MiniAOD file and a NanoAOD file.<br>
> Sometimes, we want to know what is the exact object reconstructed and used in the HLT path.<br>
> This is what `TriggerObjectStandAloneCollection` contains - the actual physics objects reconstructed at the HLT.
> 
> Have a look at the code in `plugins/SingleMuTrigAnalyzerMiniAOD.cc`, especially the analyze function starting from line 95, as well as the configuration file `ana_SingleMuMiniAOD.py`.
> 
> > ## Additional info
> > The configuration file shows that also this time we are using as input a skimmed MiniAOD file called `skim_dimu20_SingleMuon_2016G_ReReco_180k.root`. In case you are wondering again, this skim has been produced with the configuration `skim_dimu20.py`, requires two offline muons with pt above 20 GeV.
> {: .objectives}
> 
> This time we don't have a signal trigger and a separate reference trigger. Instead, we focus only on one single-muon trigger, namely HLT_IsoMu24:
> ~~~
> process.singleMuTrigAnalyzerMiniAOD.triggerName = cms.untracked.string("HLT_IsoMu24_v2")
> ~~~
> {: .language-python}
> The `SingleMuTrigAnalyzerMiniAOD.cc` analyzer is longer and more complicated than the one in [efficiency measurement Ex.][lesson-04-HLT_efficiency], and we will discuss it not only in this exercise, but also in two others that follow. So don't worry if some parts look somewhat mysterious first.
> 
> Similarly to [efficiency measurement Ex.][lesson-04-HLT_efficiency], in line 129 the name of the HLT path (`triggerName_`) is used to retrieve the corresponding index (`triggerIndex`):
> ~~~
> const unsigned int triggerIndex(hltConfig_.triggerIndex(triggerName_));
> ~~~
> {: .language-cpp}
> which is then used (in line 148) to access the HLT decision to accept or reject the event:
> ~~~
>   bool accept = triggerResultsHandle_->accept(triggerIndex);
> ~~~
> {: .language-cpp}
> You can find some new, more interesting tricks on lines 180-190. 
> There we create an empty vector `trigMuons`, loop over all trigger objects (that is, physics objects reconstructed at HLT), select the objects that HLT classified as muons that would pass our single-muon trigger, and add the four-vectors of these trigger-level muon objects to the trigMuons vector:
> ~~~
>   std::vector trigMuons;
>   if (verbose_) cout << "found trigger muons:" << endl;
>   const edm::TriggerNames &names = iEvent.triggerNames(*triggerResultsHandle_);
>   for (pat::TriggerObjectStandAlone obj : *triggerOSA_) {
>     obj.unpackPathNames(names);
>     if ( !obj.id(83) ) continue; // muon type id
>     if ( !obj.hasPathName( triggerName_, true, true ) ) continue; // checks if object is associated to last filter (true) and L3 filter (true)
>     trigMuons.push_back(LorentzVector(obj.p4()));
>     if (verbose_) cout << "  - pt: " << obj.pt() << ", eta: " << obj.eta() << ", phi: " << obj.phi() << endl;
>   } // loop on trigger objects
> ~~~
> {: .language-cpp}
> 
> Run the configuration file as follows:
> ~~~
> cd $CMSSW_BASE/src/ShortExerciseTrigger2023/ShortExerciseTrigger/test
> cmsRun ana_SingleMuMiniAOD.py 
> ~~~
> {: .language-bash}
> The code will output a file called `histos_SingleMuTrigAnalyzer.root`, which we will inspect in the next exercise.
{: .solution}

> ## NanoAOD
> Trigger objects are also stored in NanoAOD. Here are a short example of accessing 
> ~~~
> from coffea.nanoevents import NanoAODSchema
> fpath="root://cmseos.fnal.gov//store/user/cmsdas/2023/short_exercises/Trigger/New_NanoAOD_M1000.root"
> events = NanoEventsFactory.from_root(fpath,schemaclass=NanoAODSchema).events()
> ~~~
> Trigger object is one of the fields of the event
> ~~~
> trig = events.TrigObj
> trig.fields
> ['pt',
>  'eta',
>  'phi',
>  'l1pt',
>  'l1pt_2',
>  'l2pt',
>  'id',
>  'l1iso',
>  'l1charge',
>  'filterBits']
> ~~~
> The trigger objects has an `id` attribute so that you can easily find same type of trigger objects (e.g. trigger muons)
> ~~~
> trigMuon = trig[trig.id==13]
> # ID of the object: 11 = Electron (PixelMatched e/gamma), 22 = Photon (PixelMatch-vetoed e/gamma), 13 = Muon, 15 = Tau, 1 = Jet, 6 = FatJet, 2 = MET, 3 = HT, 4 = MHT
> # Note that id matches the pdgId of the particles
> ~~~
> 
> ### Filter bits
> The information of which trigger paths the trigger object has passed is stored compactly in the `filterBits` field.<br>
> The `filterBits` encoding can be found from the documentations links below. 
> 
> For example, for muons, the bits meaning is the following:
> ~~~
>   1    = TrkIsoVVL,
>   2    = Iso, 
>   4    = OverlapFilter PFTau,
>   8    = 1mu,
>   16   = 2mu,
>   32   = 1mu-1e, 
>   64   = 1mu-1tau,
>   128  = 3mu, 
>   256  = 2mu-1e,
>   512  = 1mu-2e,
>   1024 = 1mu (Mu50), 
>   2048 = 1mu (Mu100)
> ~~~
> Therefore, if you want to find trigger muons in `Iso` paths:
> ~~~
> filterBit = 2
> passIso = trigMuon.filterBits &filterBit==filterBit # mask for each trigMuon
> IsoTrigMuon = trigMuon[passIso] 
> ~~~
>
> > How can you find the trigger muons in `1mu` and `Iso` paths?
> {: .challenge}
>
> ### References
>  * [Documentation of specific version](https://cms-nanoaod-integration.web.cern.ch/autoDoc/). 
>  * [Current definitionof  trigger object in CMSSW](https://github.com/cms-sw/cmssw/blob/master/PhysicsTools/NanoAOD/python/triggerObjects_cff.py). 
{: .solution}

{% include links.md %}

