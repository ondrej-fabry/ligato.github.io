---
title: Spinnaker Overview
tags: [spinnaker]
keywords:
summary: "The Tricorder program recommends Spinnaker as the deployment tool of choice. Hence this section as a whole primarily describes Spinnaker usage."
sidebar: mydoc_sidebar
permalink: mydoc_spinnaker_overview.html
folder: mydoc
---

## Overview

Spinnaker is an open source from Netflix in partnership with Google. It is a multi-cloud continuous delivery platform for releasing software changes with high velocity and confidence

Spinnaker will take a deployable asset (e.g. a Docker image) and deploy it out though the pipeline steps, but only once it has received trigger from a separate CI platform like Jenkins

[Spinnaker site](http://www.spinnaker.io)   \\
[Spinnaker code](https://github.com/spinnaker/)

* Two core sets of features:
    * Cluster Management
    * Deployment Management

Cluster Management: Used to manage resources in the Cloud 

* Resources:
    * Server groups
    * Security Groups
    * Load Balancer
    * Cluster (logical grouping of server groups)
{% include image.html file="spinnaker1.png" %}

Deployment Management:  Used to construct and manage continuous deployment workflows
{% include image.html file="spinnaker2.png" %}

Pipeline is the key deployment management construct in Spinnaker. It is a sequence of stages, optional triggers, parameters and notifications.\\
Automatic triggers can be Jenkins job, cron or another pipeline. 

Continuous Delivery with Spinnaker:
{% include image.html file="spinnaker3.png" %}

## Spinnaker Pipeline Example:
{% include image.html file="spinnaker4.png" %}

## Spinnaker Canary Deployment:
{% include image.html file="spinnaker5.png" %}
{% include image.html file="spinnaker6.png" %}
{% include image.html file="spinnaker7.png" %}
{% include image.html file="spinnaker8.png" %}


## Spinnaker Integration Options

* Cloud Providers:
  * Amazon Web Services
  * Google Compute Platform
  * Cloud Foundry
  * Kubernetes
  * Microsoft Azure
* CI Platforms:
  * Jenkins
  * Travis
* Source Repositories
  * GitHub
  * BitBucket Server / Stash
* Messaging Support
  * Email
  * Slack
  * HipChat
  * Twilio
* Docker Registries
  * Anything with support for the v2 Docker Registry API

## Spinnaker Microservices:

* Deck is a static AngularJS-based UI.
* Gate exposes APIs for all external consumers of Spinnaker (including deck). It is the front door to Spinnaker.
* Cloud Driver integrates with each cloud provider (AWS, GCP, Azure, etc.). It is responsible for all cloud provider-specific read and write operations.
* Orca handles pipeline and task orchestration (ie. starting a cloud driver operation and waiting until it completes).
* Rosco is a packer-based bakery.  rosco provides a means to take a Debian or Red Hat package and turn it into an Amazon Machine Image. It also supports Google Compute Engine and Azure images.
* Front50 stores all application, pipeline and notification metadata.
* Igor facilitates the use of Jenkins in Spinnaker pipelines (a pipeline can be triggered by a Jenkins job or invoke a Jenkins job).
* Echo provides Spinnaker’s notification support, including integrations with Slack, Hipchat, SMS (via Twilio) and Email.

## Spinnaker versus Kubernetes Dashboard

Spinnaker targets cloud agnostic continuous deployment automation and includes support for infrastructure deployment. 

It has minimum overlap with Kubernetes Dashboard that is used for cluster management, troubleshooting of containerized application and adhoc deployment of applications. 

Good review from Google engineers on how Spinnaker and Kubernetes work together seamlessly:  
<http://blog.armory.io/why-spinnaker-and-kubernetes-work-together-seamlessly/>


## Spinnaker Advantages for Cisco

* Focus on CD. It is what we need on customer environments
* Pre-built deployment strategies – Highlander, Red/Black (Green/Blue), Canary
* API first – A single place to access pipelines and k8s resources
* Scalable – Netflix, Target
* Google developers coding the k8s integration
* Open source only option. No company behind to make money on enterprise edition or support
* Deployment as code
* Reduce the need to maintain deployment code (scripts) on customer environment
* Easy community to work with. Contribution review and check in is fast. Quick fixes.
* Consistent UI. Easy to enhance and it is one place to see it all -> pipelines and k8s resources
* Multi Cloud support. Our solutions are likely to be multi cloud in the future
* Strong Jenkins and Docker hub integration
* Easy install, configuration
* Content (pipelines) version control and export/import. Need a way to distribute/update content
* k8s enhancements – support for special controllers and deployment files - Work in progress
* Supports rollback and k8s jobs
* Easy customization


{% include links.html %}
