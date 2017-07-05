---
title: Container Networking Overview
tags: [networking]
keywords:
summary: "Cloud Native platform enables effortless deployment of a container networking solution along with the deployment of application containers. This speeds up the ovarall deployment and meets the need for container isolation and effective connectivity at the start. The Cloud Native platform enables the applications teams to use a validated networking fabric of their choice for containers on AWS, VMWare and Bare-Metal networks. 
sidebar: mydoc_sidebar
permalink: mydoc_container_net_overview.html
folder: mydoc
layout: network
---




# Cloud Native Networking

{% include image.html file="Networking-Overview.png" %}



## Background


Container based application development and deployment approach necessitates a different networking implementation than the traditional networking approach. When a container is instantiated and started, it needs a network address for connecting it to a virtual network. All containers in the system communicate with each other by directing packets to a network address (docker0 in case of Docker), which then forwards those packets through the subnet automatically. With Docker, each container is assigned an IP address that can be used to communicate with other containers on the same host. For communicating over a network, containers are tied to the IP addresses of the host machines and must rely on port-mapping to reach the desired container. This makes it difficult for applications running inside containers to advertise their external IP and port as that information is not available to them and therefor require a networking solution such as Contiv or Flannel. These networking solutions provide networking capability for containers to communicate across all containers and hosts.

The Container Networking sections provide an overview of available networking solutions, deployment steps for each networking solution, links to product pages, links to additional iformation and links to more detailed deployment guides.

Cloud Native deployment platform includes container networking deployment along with the deployment of application containers. This allows for applications to use a validated networking fabric of their choice for containers on AWS, VMWare and Bare-Metal networks. 

* `Automated deployment of container networking along with application containers`

* `Choice of validated networking options: Contiv, Flannel, Weave and Calico`

* `Choice of validated deployment platform: AWS, VMWare or Bare Metal`




* Container networking on a single host 
* Container networking within a cluster of hosts


{% include links.html %}