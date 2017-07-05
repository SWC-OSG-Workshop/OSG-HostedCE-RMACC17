---
layout: lesson
root: .
title: Hands on session

---
<div class="objectives" markdown="1">

#### Objectives
* Have jobs running on your HPC cluster by end of workshop 
* Be able to view the jobs run on cluster using the OSG's GRACC system
</div>

### HPC Cluster survey

If you haven't already, fill out the <a
href="https://goo.gl/forms/8OukxsyG6KBSGHuR2">HPC cluster questionnaire</a>. We'll use
this to setup the necessary infrastructure and configure it.

### Setting up access to your cluster

If you haven't already, you'll need to create an user account on your cluster
that will be used to submit OSG jobs.  This account can be a regular user
account.  

### Setting up SSH access and getting OSG cluster names
Once an SSH account is setup on your cluster, we'll give you an ssh pub key to
install in the `~/.ssh/authorized_keys` file for that account.  The file should
have permissions of 644 and the `~/.ssh` file should have permissions of 700.
Incorrect permissions can result in SSH refusing allow access using the supplied
public key.

In addition, we'll also provide the cluster name that we'll use to refer to your
cluster in the OSG infrastructure.  You'll need to use this in the later
sections of this tutorial. 



### Testing Integration from OSG to your HPC cluster
#### Login to submit node 

Once we've told you that the hosted infrastructure is setup, log in to the submit node:
{:class="in"}

~~~
$ ssh username@training.osgconnect.net  # username is your username
$ passwd:                            # enter your password
~~~

Go to a preloaded quickstart files

~~~
$ cd ~/tutorial-quickstart # relevant script and input files are inside this
directory
~~~

We will look at the submit file in detail: tutorial01.submit

#### HTCondor submit file

So far, so good! Let's look at a the submit file `tutorial01.submit`

    # The UNIVERSE defines an execution environment. You will almost always use VANILLA.
    Universe = vanilla
    
    # EXECUTABLE is the program your job will run It's often useful
    # to create a shell script to "wrap" your actual work.
    Executable = short.sh
    
    # ERROR and OUTPUT are the error and output channels from your job
    # that HTCondor returns from the remote host.
    Error = job.$(Cluster).$(Process).error
    Output = job.$(Cluster).$(Process).output
    
    # The LOG file is where HTCondor places information about your
    # job's status, success, and resource consumption.
    Log = job.log
    
    Requirements = (Glidein_Name == "[CLUSTER_NAME]")

    # Send the job to Held state on failure. 
    on_exit_hold = (ExitBySignal == True) || (ExitCode != 0)
    
    # Periodically retry the jobs every 1 hour, up to a maximum of 5 retries.
    periodic_release =  (NumJobStarts < 5) && ((CurrentTime - EnteredCurrentStatus) > 60*60)
    
    # QUEUE is the "start button" - it launches any jobs that have been
    # specified thus far.
    Queue 20

Here you'll need to replace the ```[CLUSTER_NAME]``` in the requirements line
with the name that we provide to you.  This will restrict the job to only run on
your cluster.  

#### Submit the job 

Submit the job using `condor_submit`:

	$ condor_submit tutorial01.submit
	Submitting job(s). 
	20 job(s) submitted to cluster 823.

#### Check the job status

The `condor_q` command tells the status of currently running jobs.

	$ condor_q netid
	-- Submitter: login01.osgconnect.net : <128.135.158.173:43606> : login01.osgconnect.net
	 ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD
	 823.0   netid           8/21 09:46   0+00:00:06 R  0   0.0  short.sh
	1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended

Note the `ST` (state) column. Your job will be in the I state (idle) if
it hasn't started yet. If it's currently scheduled and running, it will
have state `R` (running). If it has completed already, it will not appear
in `condor_q`. 

Run `condor_q` with the watch command to view the status of your jobs until it
completes (the job will be in the C state or will no longer appear in the
`condor_q` output).  Hit `Ctrl-C` to return to the command line.

#### Check the job output

Once your job submission has finished, you can look at the files that HTCondor has
returned to the working directory. If everything was successful, it
should have returned:

* a log file from HTCondor for the job cluster: jog.log
* an output file for each job's output: job.output
* an error file for each job's errors: job.error

Read the output file. It should be something like this: 

	$ cat job.output
	Start time: Wed Aug 21 09:46:38 CDT 2013
	Job is running on node: ip-12.12.12.12
	Job running as user: uid=58704(osg) gid=58704(osg) groups=58704(osg)
	Job is running in directory: /var/lib/condor/execute/dir_2120
	Sleeping for 10 seconds...
	Et voila!


### View OSG Accounting information

After your job has run, you'll need to wait about 15-20 minutes for the
accounting information to be recorded in the OSG accounting information.
Then in your web browser go to the OSG 
[accounting site](https://gracc.opensciencegrid.org/dashboard/db/payload-jobs-summary?orgId=1) 
and click on the facility dropdown.  Either scroll down and select your site
name (that we will provide) or type your site name.  You should the plots should
update and show jobs have run on your site.

<div class="keypoints" markdown="1">

#### Key Points
* HPC admins just need to add a regular user account and setup ssh pub key
* OSG job information for integrated HPC resources can be viewed using GRACC


</div>

