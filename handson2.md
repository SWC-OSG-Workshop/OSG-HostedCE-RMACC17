---
layout: lesson
root: .
title: Hands on session

---

### Background & Requirements

The Open Science Grid team is pleased to offer a new solution, "Hosted Compute
Elements", for connecting HPC clusters to the OSG via SSH. In this hands-on
session, we'll walk you through the simple steps for adding a new cluster to
OSG. 

In order to participate, you'll need a cluster that meets a few basic
requirements:
    * A supported batch system (SLURM, PBS, HTCondor, LSF, UGE) 
    * Batch system submit nodes accessible via the public internet, with SSH key-based authentication enabled
    * Outbound networking from the worker nodes (NAT is OK)

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


### Viewing Your Contribution to OSG

The OSG maintains detailed accounting information and traceability for any
end-user workload that has run on your cluster. Within 15-20 minutes of OSG jobs
submitting jobs to your cluster via SSH, you should be able to see this
information reflected
[here](https://gracc.opensciencegrid.org/dashboard/db/payload-jobs-summary?orgId=1).

Along with the OSG user's SSH public key, you should have also received
information about your "site name" within the accounting system. If you click
the "Facility" dropdown, you should be able to type this site name in and see
job information such as wall hours arranged by field of science, organization,
project etc.


### Other Recommended Software

#### OASIS

Many users of the Open Science Grid depend on the "OSG Application
Software Installation Service", or OASIS. 

OASIS relies on CVMFS - an HTTP-based, FUSE filesystem for distributing
software and data. In order to support OASIS, the OASIS tools must be installed
on all worker nodes. Additionally, all workers need to the FUSE module enabled
in their OS kernel. FUSE is enabled by default in Red Hat variants.

For more information, see
[here](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/UpdateOasis)

#### Singularity

To easily distribute scientific software, many users are now
turning to containerization technologies. On the Open Science Grid, a large
number of facilities now support a lightweight containerization technology
called Singularity. Singularity contains a single setuid (root-privileged)
binary and no long-running services, making it ideal for running containerized
workloads on HPC clusters.

For sites running Red Hat variants, the Open Science Grid provides Singularity
RPMs in the OSG repository. On EL7:

```bash 
rpm -Uhv https://repo.grid.iu.edu/osg/3.4/osg-3.4-el7-release-latest.rpm 
yum install singularity 
```

And for EL6:

```bash
rpm -Uhv https://repo.grid.iu.edu/osg/3.4/osg-3.4-el6-release-latest.rpm
yum install singularity 
```

For more information, see [here](http://singularity.lbl.gov/)
