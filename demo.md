---
layout: lesson
root: .
title:  OSG / HPC resource integration demonstration

---
<div class="objectives" markdown="1">

#### Objectives
* Show the steps needed to integrate a HPC cluster with OSG's job scheduling
* Demonstrate what occurs with login accounts as OSG staff integrate an HPC resource
* Demonstrate how to view accounting information for OSG jobs running on your cluster
</div>

## Introduction

This page will briefly outline the steps HPC resource admins and OSG staff will
need to take in order to integration a HPC resource into the OSG infrastructure.

## Steps taken on a HPC resource being integrated
   * Create new user on HPC resource that will be used to submit OSG jobs
   * Add ssh public key provided by OSG staff

## Steps taken by OSG staff
   * Configure hosted OSG software with HPC resource specifics
   * Generate ssh key to be used to login to HPC resource
   * Log in to HPC resource and setup OSG software in user account
   * Add HPC resource information to OSG information and job management systems
   * Test and validation job submission and execution through OSG systems

## Getting information on jobs run on HPC resource
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

