---
layout: lesson
root: .
title: Hands on session

---

### Background & Requirements

The Open Science Grid team is pleased to offer a new solution, "Hosted Compute
Elements" (or Hosted CE), for connecting HPC clusters to the OSG via SSH. In
this hands-on session, we'll walk you through the simple steps for adding a new
cluster to OSG. 

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
key to install in the user's home directory. On normal systems, this key should
be pasted into `~<osg-user-name>/.ssh/authorized_keys`, where <osg-user-name> is
the username you selected for the OSG user, without the brackets. Typically,
the `.ssh` directory should have permissions of `700 (rwx------)` and
the `authorized_keys` file should have permissions of `644 (rw-r--r--)`. The key
should *not* contain line breaks.


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
