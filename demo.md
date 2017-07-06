---
layout: lesson
root: .
title:  Campus HPC resource integration with the OSG: demonstration

---

<div class="objectives" markdown="1">

#### Objectives
* Show the steps needed to integrate a campus HPC cluster with OSG's job scheduling systems.
* Demonstrate what occurs with login accounts as OSG staff integrate an HPC resource.
* Demonstrate how to view accounting information for OSG jobs running on your cluster.

</div>

## Introduction

We briefly outline the steps HPC resource administrators and OSG staff will
take in order to integrate a HPC resource into the OSG high throughput ecosystem.

## Steps taken by campus HPC resource administrator
   * Create new user account on HPC resource that will be used to submit OSG jobs
   * Add SSH public key provided by OSG staff
   * Communicate to OSG staff when user account is ready to go

## Steps taken by OSG staff
   * Configure hosted OSG software with specifics of the campus HPC cluster
   * Generate SSH key to be used to login to HPC resource
   * Log in to HPC resource and setup OSG software in the OSG user account
   * Add the new HPC resource information to OSG information and job management systems
   * Test and validate job submission and execution through OSG systems

## Getting information on jobs run on the HPC resource
   * Visit [OSG accounting site](https://gracc.opensciencegrid.org/dashboard/db/payload-jobs-summary?orgId=1)
   * Select appropriate time range 
   * Select correct facility name
   * Information available includes projects, institutions, and fields of science using resource


<div class="keypoints" markdown="1">
#### Key Points
* HPC admins just need to add a regular user account and setup ssh pub key
* OSG staff add wn-client software to the user account 
* OSG job information for integrated HPC resources can be viewed using GRACC
</div>

