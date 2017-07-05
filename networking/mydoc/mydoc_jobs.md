---
title: Tricorder Jobs
tags: [spinnaker]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_jobs.html
folder: mydoc
---

## What are Tricorder jobs?

Tricorder jobs are used to execute atomic functions. They can be executed as Spinnaked jobs or Kubernetes jobs.\\
They are short lived pods that execute a specific functions that can help on operation , configuration and tests


## Repo

Tricorder jobs are at:
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-jobs/browse>

They are built via Jenkins job <https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Job_Images_Build/> and pushed to Artifactory.


## Available jobs

We intent to have a catalog of jobs. The jobs available now are:

* kubectl - Allows the execution of kubectl commands.
* healthcheck - Monitors pod status.



{% include links.html %}
