---
title: "HLT timing studies"
teaching: 30
exercises: 0
objectives:
- "Measure the time it takes to run an HLT menu"
- "Reference repo: [link](https://github.com/loeschet/TimingExerciseTriggerHATS2023/blob/main/README.md)"
---

### Exercise 1: CPU/GPU Timing measurements without creation of a CMSSW environment

1. Log in to lxplus and clone [the timing repository](https://gitlab.cern.ch/cms-tsg/steam/timing) somewhere (e.g. in your EOS space)

~~~
git clone https://gitlab.cern.ch/cms-tsg/steam/timing.git
cd timing
~~~
{: .language-bash}

2. Submit a timing job to the timing machine using `CMSSW_13_2_0`, the GRun menu V152 and the default dataset on the timing machine. Additionally, tell the program to merge in pull request [#42534](https://github.com/cms-sw/cmssw/pull/42534) Also use a user-specified tag to better identify your job later in the queue.

~~~
python3 submit.py /dev/CMSSW_13_0_0/GRun/V152 --cmssw CMSSW_13_2_0 --pull-requests 42534 --tag YOUR_TAG_HERE
~~~
{: .language-bash}

3. Check the status of your job using the `job_manager.py` script.

~~~
python3 job_manager.py
~~~
{: .language-bash}


{% include links.md %}

