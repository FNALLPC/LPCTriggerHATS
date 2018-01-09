# HLTDAS2018
HLT trigger exercise CMS DAS @ Fermilab LPC, January 2018

See DAS twiki for detailed instructions:
https://twiki.cern.ch/twiki/bin/view/CMS/SWGuideCMSDataAnalysisSchoolLPC2018TriggerExercise

## Jupyter noteboooks

If you want to use Jupyter notebooks on cmslpc, please review the content of your `~/.ssh/config` file on your system by executing:

    cat ~/.ssh/config

In case the file does already contain the following lines, add:

    Host cmslpc*.fnal.gov
        StrictHostKeyChecking no
        UserKnownHostsFile /dev/null

When you log into cmslpc, add a `-L` option to your ssh command:

    ssh -L localhost:8888:localhost:8888 <YOUR USERNAME>@cmslpc-sl6.fnal.gov

Then you can make your area
    
    cmsrel CMSSW_9_2_15
    cd CMSSW_9_2_15/src
    cmsenv

And start Jupyter with this command:

    jupyter notebook --no-browser --port=8888 --ip 127.0.0.1

After a pause (while cmslpc loads the necessary libraries for the first time) you should see a message like the following:

    [I 08:22:45.871 NotebookApp] Serving notebooks from local directory: /uscms_data/d2/pivarski/CMSSW_9_0_0_pre6/src
    [I 08:22:45.871 NotebookApp] 0 active kernels 
    [I 08:22:45.871 NotebookApp] The Jupyter Notebook is running at: http://localhost:8888/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    [I 08:22:45.871 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 08:22:45.873 NotebookApp] 
        
        Copy/paste this URL into your browser when you connect for the first time,
        to login with a token:
            http://localhost:8888/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
