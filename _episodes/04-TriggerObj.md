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
> trigMuon = trig[trig.id==15]
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
> passIso = trigMuon.filterBits &(1<<filterBit)==(1<<filterBit) # mask for each trigMuon
> IsoTrigMuon = trigMuon[passIso] 
> ~~~
>
> ### References
> [Documentation of specific version](https://cms-nanoaod-integration.web.cern.ch/autoDoc/). 
> [Current definitionof  trigger object in CMSSW](https://github.com/cms-sw/cmssw/blob/master/PhysicsTools/NanoAOD/python/triggerObjects_cff.py). 
{: .solution}

{% include links.md %}

