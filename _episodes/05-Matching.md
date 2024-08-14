---
title: "Matching trigger objects"
teaching: 10
exercises: 0
objectives:
- "Learn how to access the trigger information stored in MiniAOD and NanoAOD"
- "Learn what is trigger objects and how to access them"
- "Measure trigger efficiency using the tag-and-probe method"
---

## Match trigger objects to offline objects

Trigger objects are physics objects(electrons,muons,jets,MET..etc) reconstructed at HLT and are used for making HLT decisions of each HLT path.
Offline objects are reconstructed offline, which in general has more precise reconstructions.
If we want to know whether an offline object(e.g. a muon) is responsible for firing the HLT, we can look for a trigger muon within a certain `dR` of the offline muon.

### Collection names 
We can learn from the example in the `analyze` function in `SingleMuTrigAnalyzerMiniAOD.cc.`<br>
The offline objects are reconstructed primary vertices (offlineSlimmedPrimaryVertices) and offline-reconstructed muons (slimmedMuons).<br>
The trigger object is `trigMuons`, which we obtained from the `TriggerObjectStandAloneCollection` from last episode.<br>
Here we are not interested in primary vertices per se, we just need to get them because later we will use them in identification of "tight" muons.

### Matcing by momentum direction `dR`

> Take a look at lines 231-286 in SingleMuTrigAnalyzerMiniAOD.cc, where you can find two for loops that iterate over the offline muons.
{: .callout}

Since the second for loop is inside the first one, together these two for loops are going over all possible pairs of offline muons. 
The first muon in each pair is called "tag", and the second is called "probe", for reasons that will soon become apparent. 
Inside the loop, several selection criteria are placed on muons, such as cuts on pt and eta, tight identification, and isolation requirements.

Now that we have both trigger-level muons and offline-reconstructed muons, we can compare them with each other. 
In order to do this, we need to figure out which trigger muon corresponds to which offline muon. This is called *trigger matching*.
In the code, trigger matching is performed both for tag muons ([lines 245-248](https://github.com/FNALLPC/LPCTriggerHATS/blob/master/ShortExerciseTrigger/plugins/SingleMuTrigAnalyzerMiniAOD.cc#L245-L248)) and probe muons ([lines 277-280](https://github.com/FNALLPC/LPCTriggerHATS/blob/master/ShortExerciseTrigger/plugins/SingleMuTrigAnalyzerMiniAOD.cc#L277-L280)). 
For tag muons, the matching code looks like this:
~~~
   bool trigmatch_tag = false;
   for (unsigned int itrig=0; itrig < trigMuons.size(); ++itrig) {
      if (ROOT::Math::VectorUtil::DeltaR(muon_tag->p4(),trigMuons.at(itrig)) < dr_trigmatch) trigmatch_tag = true;
   }
~~~
Since reconstruction of muon trajectory is less precise in HLT than in offline reconstruction, there might be a small difference between the directions of the trigger muon and the offline muon, even if they correspond to the same original real muon. 
Therefore a DeltaR cone of 0.2 is used to geometrically match the trigger object and the offline object (`dr_trigmatch = 0.2`).

Finally, if the two muons pass the selections in the code, and the tag muon passes the trigger matching, the invariant mass of this dimuon system is calculated and saved in a histogram:
~~~
     LorentzVector dimuon = LorentzVector(muon_tag->p4() + muon_probe->p4());
     hists_1d_["h_mll_allpairs"]->Fill(dimuon.M());
     if (verbose_) cout << " - probe dimuon mass: " << dimuon.M()  << endl;
     if (dimuon.M() < 81. || dimuon.M() > 101.) continue;
     hists_1d_["h_mll_cut"]->Fill(dimuon.M());
~~~

> Use the `TBrowser` to open the file `histos_SingleMuTrigAnalyzer.root` and have a look at the histogram `h_mll_allpairs`, which shows the dimuon invariant mass distribution of muon pairs that passed the selections. 
> Can you explain the shape of the distribution?
{: .challenge}

{% include links.md %}

