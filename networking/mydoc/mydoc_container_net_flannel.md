---
title: Container Networking - Flannel
tags: [networking]
keywords:
summary: "Flannel is a virtual network that gives a subnet to each host for use with container runtimes. Flannel provides each container an IP that can be used for container-to-container communication. "
sidebar: mydoc_sidebar
permalink: mydoc_container_net_flannel.html
folder: mydoc
layout: network
foldertitle: Networking
---

{% include image.html file="Networking-Flannel.png" %}


Flannel is an open source project that provides a Container networking solution and can be used with Kubernetes to set up networking between container and pods. Flannel allocates a separate subnet for every host where a Container runs, and the Containers in this host get allocated an individual IP address from the host subnet. An overlay network is set up between each host that allows Containers on different hosts to talk to each other. Flannel gives each container an IP that can be used for container-to-container communication. It uses packet encapsulation to create a virtual overlay network that spans the whole cluster. More specifically, flannel gives each host an IP subnet (/24 by default) from which the Docker daemon is able to allocate IPs to the individual containers. flannel uses etcd store mappings between the virtual IP and host addresses. A flanneld daemon runs on each host and is responsible for watching information in etcd and routing the packets. 

[Flannel web page](https://github.com/coreos/flannel)

[Integrating Flannel via Kubenetes Addon](https://kubernetes.io/docs/admin/addons/)

## Deployment

### Automated Deployment
Flannel will be deployed as a component of the overall envrironment configuration. Each system deployment's environemnt is specific to a customer and/or application requirements. Therefor, a method is provided for choosing a networking option in tbe environment build autoamtion. 

The users will define the choice of networking fabric in one step by setting an environment variable in the "tricorder-environments" file.

1. Set environment variable in envrionments ".cfg" file as below:

	`NETWORKING_OPTION=Flannel`  
	
	  [Example Configuration File](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-environments/browse/prod/vsphere/tenants/example/example.cfg)

### Manual Deployment 

Flannel can be deployed manually through the following steps: 
 
1. Assumes Docker and Kubenetes have been deployed2. 	Initialize the Master with the following command
	`kubeadm init --pod-network-cidr=10.244.0.0/16`
3. Deploy Flannel
	`kubectl create -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml`
	
## Additional information

Please refer to the below deployment guide for:

1. *Detailed deployment instructions* 
2. *Steps to verify networking deployment*
 
	[Networking Deployment Guide](https://cisco.jiveon.com/docs/DOC-1702651)



{% include links.html %}
