---
title: Devops 1.0
tags: [devops]
keywords: devops
last_updated: March 26, 2017
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_devops10.html
folder: mydoc
---

## Overview

This document covers the available components of the Tricorder CI/CD process. All documentation is in markdown files within the repos related to Tricorder CI/CD.

{% include image.html file="devops.png"  alt="DevOps" caption="DevOps" %}

 

## High Level use cases for CI/CD

See <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/highlevel_overview.md>


## Build

### Infrastructure

Infrastructure Base Images:\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse>

Jenkins Job to Build Base Images:\\
<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Base_Images_Build/>

### Java Base Classes & Support Microservices

Java Base Class - Logging and Service Invocation:\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-base-java/browse>

Java Base Class - Cache (Redis):\\
 <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-cache-java/browse>

Java Base Class - Messaging (Kafka):\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-messaging-java/browse>

Java Base Class - Tracing (Hystrix):\\
 <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-tracing-java/browse>

Java Support Microservice - CSRF (Cross Site Request Forgery):\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-csrf-service/browse>


Java Support Microservice - Sample Java Microservice:\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-java/browse>


### Java Service build Example

Java Sample Microservice includes build instructions:\\
 <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-java/browse>

Jenkins Job to Build Sample Java Microservice:\\
<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Sample_Java_Service_Build/>


### Go Base Packages  & Support Microservices

Go Base - Logging and Service Invocation:\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-base-go/browse>

Go Base  -  key-value store (etcd):\\
 <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-base-go/browse/etcd>

Go Base - Messaging (Kafka):\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-messaging-go/browse>

Go Support Microservice - Sample Go Microservice:\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-go/browse>


### Go Service build Example

Go Sample Microservice includes build instructions:\\
 <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-go/browse>

Jenkins Job to Build Sample Go Microservice:\\
<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Sample_Go_Service_Build/>


### C Base Class & Support Microservice
  
C Integration with Java Base Classes via JNI://
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-base-jni/browse>

Base Container for C application://
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-c/browse>


## Deployment

### Environment Configuration and BOM

The Tricorder Environments config contains environment's individual configuration and connection information as well as a reference to the Bill of Materials (BoM).\\
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-environments/browse>

A Tricorder BoM is a list of the "What" and the "How" components and services will be deployed in an environment. The "What" is the list of artifacts that will be deployed. The "How" is bootstrap and deployment code pinned to a particular repo version tag.
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bom/browse>

{% include image.html file="bom.png"  alt="BOM" caption="BOM and Environment Configuration Example" %}


### Bootstrap the Environment and Deploy Components:

<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse>

Jenkins Job to Deploy Components:\\
<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Deploy_Components/>


Tricorder deploys the following support components in Kubernetes to be used by application services:


|  Component | Version  | Image Build | Deployment Role
|  --------- | -----------  |  ---- |
|  Cassandra | 3.10 | [Cassandra image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/cassandra)  | [Cassandra deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/cassandra)
|  Elk  | 5.1.2 | [elk images](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/elasticsearch) [doc](http://elk-docker.readthedocs.io/) | [elk deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/elasticsearch)
|  Etcd | 3.1.3 | [etcd image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/etcd) | [etcd deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/etcd)
|  Fluentd | 1.1 | [fluentd image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/fluentd) | [fluentd deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/fluentd)
|  Kafka | 0.10.1.1 | [kafka image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/kafka)  | [kafka deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/kafka)
|  Zookeeper | 3.4.9 |  [zookeeper image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/zookeeper) | [zoopeeper deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/kafka/templates)
|  Redis | 2.8.19 | [redis image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/redis) | [redis deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/redis)
|  VPP | 17.04 | [vpp image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/tric_vpp_vnf_agent)  | [vpp deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/tric_vpp_vnf_agent)
|  Zipkin | 1.20 | [zipkin image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/zipkin) | [zipkin deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/zipkin)
| Kubernetes | 1.5.4 |  | [kubernetes deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/kubernetes) 



### Deploy Services (Applications)

<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse>

Jenkins Job to Deploy Services:\\
<https://engci-private-sjc.cisco.com/jenkins/spo/job/Tricorder/job/Dev/job/Tricorder_Deploy_Service/>


## Support

Tricorder uses CiscoSpark for real time support and collaboration. There is a team space on spark here: <https://web.ciscospark.com/teams/f82e8c00-003b-11e7-9dd2-fffe351f5a21>

E-mail tricorder-admins@cisco.com with your user ID requesting admission to the spark room

{% include links.html %}
