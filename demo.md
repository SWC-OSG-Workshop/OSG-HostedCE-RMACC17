---
layout: lesson
root: .
title: Campus HPC resource integration with the OSG demonstration

---

## Introduction

We briefly outline the process for integrating a campus HPC cluster with the OSG high throughput ecosystem.

## Steps taken by the campus HPC resource administrator
   * Create new user account on HPC resource that will be used to submit OSG jobs
   * Add SSH public key provided by OSG staff
   * Communicate to OSG staff when user account is ready to go

## Steps taken by OSG staff
   * Configure the managed OSG software with specifics of the campus HPC cluster
   * Generate SSH key to be used to login to the campus HPC resource
   * Log in to the campus HPC resource and setup OSG software in the OSG user account
   * Add the new campus HPC resource information to the OSG information and job management systems
   * Test and validate job submission and execution through OSG systems

## Viewing campus HPC resource contributions
   * Visit the [OSG accounting site](https://gracc.opensciencegrid.org/dashboard/db/payload-jobs-summary?orgId=1)
   * Select an appropriate time range 
   * Select the facility name of the HPC cluster
   * Information available includes projects, institutions, and fields of science using resource

