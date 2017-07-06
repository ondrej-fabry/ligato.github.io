---
title: Container Networking Overview
overview: Information on the various ways to participate and interact with the Ligato community.
layout: network 
type: markdown
sidebar: mydoc_sidebar
---

{% include image.html file="Networking-Overview.png" %}



## Background


Container based application development and deployment approach necessitates a different networking implementation than the traditional networking approach. When a container is instantiated and started, it needs a network address for connecting it to a virtual network. All containers in the system communicate with each other by directing packets to a network address (docker0 in case of Docker), which then forwards those packets through the subnet automatically. With Docker, each container is assigned an IP address that can be used to communicate with other containers on the same host. For communicating over a network, containers are tied to the IP addresses of the host machines and must rely on port-mapping to reach the desired container. This makes it difficult for applications running inside containers to advertise their external IP and port as that information is not available to them and therefor require a networking solution such as Contiv or Flannel. These networking solutions provide networking capability for containers to communicate across all containers and hosts.

The Container Networking sections provide an overview of available networking solutions, deployment steps for each networking solution, links to product pages, links to additional iformation and links to more detailed deployment guides.

## Capabilities

Cloud Native deployment platform includes container networking deployment along with the deployment of application containers. This allows for applications to use a validated networking fabric of their choice for containers on AWS, VMWare and Bare-Metal networks. 

**Container networking deployment delivers the following user features:**

* `Automated deployment of container networking along with application containers`

* `Choice of validated networking options: Contiv, Flannel, Weave and Calico`

* `Choice of validated deployment platform: AWS, VMWare or Bare Metal`



The container networking will be deployed along with other base components and services. The networking deployment steps will be included in the bootstrapping phase. 

Initially, the deployment and the associated documentation will cover Contiv, Flannel, Weave and Calico. Other networking options will be included in future iterations.

**The following deployment models are supported on AWS, VMWare and Bare Metal:**

* Container networking on a single host 
  * One host with Docker default networking
  * One host with Docker, Kubernetes and a networking solution
* Container networking within a cluster of hosts
  * Multiple hosts with Docker, Kubernetes and a networking solution
* External connectivity 
  * Container within the cluster exposed as a service to outside 


{% include links.html %}
