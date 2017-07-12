---
title: Container Networking - Calico
tags: [networking]
keywords:
summary: "Calico provides virtual networking and network security for containers. Calico provides a flat IP network that can typically be run without any encapsulation (no overlays). "
sidebar: mydoc_sidebar
permalink: mydoc_container_net_calico.html
folder: mydoc
layout: network
foldertitle: Networking
---

{% include image.html file="Networking-Calico.png" %}


Calico implements a pure Layer 3 approach to achieve a simple and efficient multi-host networking. Calico cannot be treated as an overlay network. The pure Layer 3 approach avoids the packet encapsulation associated with the Layer 2 solutions. Calico also implements BGP protocol for routing combined with a pure IP network, thus allows scaling for virtual networks. In addition Calico's network fabric includes the ability to route packets using a stateless IP-in-IP overlay for any scenarios where an overlay network might be preferred.

[Calico web page](https://www.projectcalico.org//)

[Integrating Calico via Kubenetes Addon](https://kubernetes.io/docs/admin/addons/)


## Deployment

### Automated Deployment
Calico will be deployed as a component of the overall envrironment configuration. Each system deployment's environemnt is specific to a customer and/or application requirements. Therefor, a method is provided for choosing a networking option in tbe environment build autoamtion. 

The users will define the choice of networking fabric in one step by setting an environment variable in the "tricorder-environments" file.

1. Set environment variable in envrionments ".cfg" file as below:

	`NETWORKING_OPTION=Calico`
	
	  [Example Configuration File](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-environments/browse/prod/vsphere/tenants/example/example.cfg)

### Manual Deployment 

Calico can be deployed manually through the following steps: 

1. Assumes Docker and Kubenetes have been deployed2. 	Initialize the Master with the following command	
	`kubeadm init`
3. Deploy Calico

	`Flannelkubectl create - f http://docs.projectcalico.org/v2.0/getting-started/kubernetes/installation/hosted/kubeadm/calico.yaml`	

## Additional information

Please refer to the below deployment guide for:

1. *Detailed deployment instructions* 
2. *Steps to verify networking deployment*
 
	[Networking Deployment Guide](https://cisco.jiveon.com/docs/DOC-1702651)


{% include links.html %}
