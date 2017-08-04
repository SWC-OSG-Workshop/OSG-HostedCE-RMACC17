---
layout: lesson
root: .
title: Hands on session

---

*Site Progress [Matrix](https://docs.google.com/spreadsheets/d/1z9iEh5kDW3YvjXmqVRtoYGfnP15L5M5TuFLuw68hzqM/edit?usp=sharing)*


### Background & Requirements

The Open Science Grid team is pleased to offer a new solution, "Hosted Compute
Elements", for connecting HPC clusters to the OSG via SSH. In this hands-on
session, we'll walk you through the simple steps for adding a new cluster to
OSG. 

In order to participate, you'll need a cluster that meets a few basic
requirements:

- A supported batch system (SLURM, PBS, HTCondor, LSF, SGE) 
- Batch system submit nodes accessible via the public internet, with SSH key-based authentication enabled
- Outbound networking from the worker nodes (NAT is OK)

### HPC Cluster Questionnaire

To streamline the process of connecting your cluster, we have created a <a
href="https://goo.gl/forms/8OukxsyG6KBSGHuR2">HPC Cluster Questionnaire</a> that
will enable the OSG team to quickly configure and customize the job submission
software for your site.

If you haven't already, please take the time to fill out this survey now. If you
are unsure about certain answers or have questions, we'll be happy to help.

### OSG User Setup

As noted above, the OSG job submission software will need to have SSH access to
your cluster in order to submit workloads. At this time, we only support
key-based authentication. Other authentication methods such as password-only,
MFA or 2FA will not work. 

The OSG user should be created in the same way as any other user on your system.
If you have filled out the HPC cluster questionnaire, please use the same
username as specified there. Otherwise, let us know what username you would like
us to use.

Once the OSG user is setup, please let us know and we'll send you an SSH public
key to install in the user's home directory. On typical systems, this key
should be pasted into `~[osg-user-name]/.ssh/authorized_keys`, where
`[osg-user-name]` is the username you selected for the OSG user, without the
brackets. The key should *not* contain line breaks.

You may need to verify that the `.ssh` directory and `authorized_keys` file
have the correct permissions.

Once the OSG team has validated that we can log in and submit jobs, sit back
and relax while we configure your site!

### Viewing Your Contribution to OSG

The OSG maintains detailed accounting information and traceability for any
end-user workload that has run on your cluster. Within 15-20 minutes of OSG 
submitting jobs to your cluster via SSH, you should be able to see this
information reflected
[here](https://gracc.opensciencegrid.org/dashboard/db/payload-jobs-summary?orgId=1).

Along with the OSG user's SSH public key, you should have also received
information about your "site name" within the accounting system. If you click
the "Facility" dropdown, you should be able to type this site name in and see
job information such as wall hours arranged by field of science, organization,
project etc.


### Other Recommended Software (optional, post workshop)

#### OASIS

Many users of the Open Science Grid depend on the "OSG Application
Software Installation Service", or OASIS. 

OASIS relies on CVMFS - an HTTP-based, FUSE filesystem for distributing
software and data. In order to support OASIS, the OASIS tools must be installed
on all worker nodes, and AutoFS must be available. Additionally, all workers
need to the FUSE module enabled in their OS kernel. FUSE is enabled by default
in Red Hat variants.

For performance, OASIS utilizes caching where-ever possible. In order to deploy
OASIS at your site, you will first need a Squid server. We recommend installing
a virtual machine (or physical hardware) with:

- Enterprise Linux 6 or 7 (Red Hat, Scientific Linux, CentOS or other variants)
- At least 2 CPUs, 4GB RAM, and 100GB disk
- Ports 3128/tcp and 3401/udp open in your firewall, if necessary.

We will assume EL6 for the rest of the document. These instructions should also
work for EL7, but you will need to adjust the RPMs appropriately (e.g.,
osg-3.4-el6-release-latest.rpm becomes osg-3.4-el7-release-latest.rpm).
Likewise, you'll need to replace the `chkconfig` and `service` commands with the
systemd equivalents.

First, you will need to install EPEL and the OSG repositories if you have not
already:
```bash
# yum install epel-release
# yum install yum-plugins-priorities
# yum install https://repo.grid.iu.edu/osg/3.4/osg-3.4-el6-release-latest.rpm
```    

Once complete, you'll need to install the Squid software, enable, and start it:
```bash
# yum install frontier-squid
# chkconfig frontier-squid on
# service frontier-squid start
```

To test that Squid is working properly, you can try the following:
```bash
$ export http_proxy=http://yoursquid.your.domain:3128
$ wget -qdO/dev/null http://frontier.cern.ch 2>&1|grep X-Cache
X-Cache: MISS from yoursquid.your.domain
$ wget -qdO/dev/null http://frontier.cern.ch 2>&1|grep X-Cache
X-Cache: HIT from yoursquid.your.domain
```

where "yoursquid.your.domain" is the fully-qualified domain name of your Squid
server.

Once working, please forward this information to us at
user-support@opensciencegrid.org, so that we may adjust the job submission
software to take advantage of this cache.

In addition, you may want to customize your Squid configuration.  To do this,
you'll need to modify `/etc/squid/customize.sh` and make the appropriate changes
there. [This link](https://twiki.cern.ch/twiki/bin/view/Frontier/InstallSquid#Configuration)
has information on customizing the most important parameters (cache sizes and
machines allowed to use Squid). 

To set up access to the OASIS filesystem, you'll need to install EPEL and the
OSG repositories to your worker nodes, as before:
```bash
# yum install epel-release
# yum install yum-plugins-priorities
# yum install https://repo.grid.iu.edu/osg/3.4/osg-3.4-el6-release-latest.rpm
```

And finally the OASIS software:
```bash
# yum install osg-oasis
```

To configure OASIS, first create or edit `/etc/fuse.conf` and add the following:
```
user_allow_other
```

Then, add the CVMFS path to `/etc/auto.master`:
```
/cvmfs /etc/auto.cvmfs
```

AutoFS will need to be started or restarted after this. 
```
service autofs restart
```

You will need to configure CVMFS such that it downloads files through your
Squid cache set up in the previous section. You will also need to set a maximum
quota for the worker's local CVMFS cache. Create the file
`/etc/cvmfs/default.local` and add the following:
```
CVMFS_REPOSITORIES="`echo $((echo oasis.opensciencegrid.org;echo cms.cern.ch;ls /cvmfs)|sort -u)|tr ' ' ,`"
CVMFS_QUOTA_LIMIT=20000
CVMFS_HTTP_PROXY="http://yoursquid.your.domain:3128"
```

where `yoursquid.your.domain` is the fully qualified domain name of your Squid
service. We assume a cache of 20000 MB -- please adjust this value as you see
fit.

To test CVMFS, try listing the contents of the OASIS server:
```
$ ls /cvmfs/oasis.opensciencegrid.org
atlas       csiu         geant4    gm2      ligo           nanohub          sbgrid
auger       current   glastorg  icecube  lsst           nova         snoplussnolabca
belle       enmr         glow      ilc      microboone  osg          uc3
cmssoft  fermilab  gluex     lbne     mis            osg-software     wastebin
```

If you see output similar to the above, you're all set!


For more information or troubleshooting, please see:

* [https://twiki.grid.iu.edu/bin/view/Documentation/Release3/InstallFrontierSquid](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/InstallFrontierSquid)
* [https://twiki.opensciencegrid.org/bin/view/Documentation/Release3/InstallCvmfs](https://twiki.opensciencegrid.org/bin/view/Documentation/Release3/InstallCvmfs)



#### Singularity

To easily distribute scientific software, many users are now
turning to containerization technologies. On the Open Science Grid, a large
number of facilities now support a lightweight containerization technology
called Singularity. Singularity contains a single setuid (root-privileged)
binary and no long-running services, making it ideal for running containerized
workloads on HPC clusters.

For sites running Red Hat variants, the Open Science Grid provides Singularity
RPMs in the OSG repository. On EL6:

```bash
# rpm -Uhv https://repo.grid.iu.edu/osg/3.4/osg-3.4-el6-release-latest.rpm
# yum install singularity 
```

And for EL7:

```bash 
# rpm -Uhv https://repo.grid.iu.edu/osg/3.4/osg-3.4-el7-release-latest.rpm 
# yum install singularity 
```

For more information, see [here](http://singularity.lbl.gov/)
