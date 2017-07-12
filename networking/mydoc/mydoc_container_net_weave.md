---
title: Container Networking - Weave
tags: [networking]
keywords:
summary: "Weave creates a virtual network that connects Docker containers deployed across multiple hosts. To application containers, the network established by Weave resembles an Ethernet switch, where all containers are connected and can easily access services from one another."
sidebar: mydoc_sidebar
permalink: mydoc_container_net_weave.html
folder: mydoc
layout: network
foldertitle: Networking
---

{% include image.html file="Networking-Weave.png" %}


Weave allows the containers on a Weave network to use standard port numbers (e.g. MySQL’s default is port 3306). Every container can find the IP of any other container using a simple DNS query on the container’s name, and it can also communicate directly without NAT or using port mappings. Weave has two different connection modes. One is called sleeve, which implements a UDP channel to traverse IP packets from containers. Weave will merge multiple containers’ packets to one packet and transfer via UDP channel, so technically Weave sleeve mode will be a bit faster than other networking plugins that use UDP like Flannel mode in most cases. The other connection mode of Weave is called fastdp mode, which implements a VxLAN solutions. Weave runs a Docker container performing the same role as flanneld does for Flannel. Weave uses the traditional CIDR isolation which uses netmask to identify different subnet, and machines in different subnet which cannot talk to each other.

[Weave web page](https://www.weave.works/)

[Integrating Weave via Kubenetes Addon](https://kubernetes.io/docs/admin/addons/)

## Deployment

The Cloud Native platform enables automated deployment of the networking along with the environment and application containers. The below sections describe the automated and manual deployement options. Users should take advantage of the automated deployment to speed up the deployment and to validate container connectivity and policy enforcement. The following sections describe the required deployment steps for AWS, WMWare and Bare Metal using Contiv.
### Automated Deployment
Weave will be deployed as a component of the overall envrironment configuration. Each system deployment's environemnt is specific to a customer and/or application requirements. Therefor, a method is provided for choosing a networking option in tbe environment build autoamtion. 

The users will define the choice of networking fabric in one step by setting an environment variable in the "tricorder-environments" file.

1. Set environment variable in envrionments ".cfg" file as below:

	`NETWORKING_OPTION=Weave`  
	
	  [Example Configuration File](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-environments/browse/prod/vsphere/tenants/example/example.cfg)

### Manual Deployment 

Weave can be deployed manually through the following steps:  

1. Assumes Docker and Kubenetes have been deployed2. 	Initialize the Master with the following command

    `kubeadm init`

3. Deploy Weave

	`kubectl create -f https://git.io/weave-kube`
	
## Additional information

Please refer to the below deployment guide for:

1. *Detailed deployment instructions* 
2. *Steps to verify networking deployment*
 
	[Networking Deployment Guide](https://cisco.jiveon.com/docs/DOC-1702651)


{% include links.html %}
