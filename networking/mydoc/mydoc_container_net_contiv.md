---
title: Container Networking - Contiv
tags: [networking]
keywords:
summary: "Contiv unifies containers, VMs, and bare metal with a single networking fabric, allowing container networks to be addressable from VM and bare-metal networks. Contiv combines strong network performance, support for industry-leading hardware, and an application-oriented policy that can move across networks with the application."
sidebar: mydoc_sidebar
permalink: mydoc_container_net_contiv.html
folder: mydoc
layout: network
---

{% include image.html file="Networking-Contiv.png" %}


Contiv Netplugin is a generic networking plugin that is designed to provide multi host policy based networking for containerized applications. Contiv Netplugin is designed to be used out-of-the-box with Docker containers and with cluster schedulers such as Kubernetes. This networking option provides for an approach to multi host networking solution independent of the scheduler platform. Contiv works with different kinds of networks like pure layer 3 networks, overlay networks, and layer 2 networks, and provides the same virtual network view to containers regardless of the underlying technology. 

[Contiv web page](http://contiv.github.io/)

[Integrating Contiv via Kubenetes Addon](https://kubernetes.io/docs/admin/addons/)

## Deployment
The Cloud Native platform enables automated deployment of the networking along with the environment and application containers. The below sections describe the automated and manual deployement options. Users should take advantage of the automated deployment to speed up the deployment and to validate container connectivity and policy enforcement. The following sections describe the required deployment steps for AWS, WMWare and Bare Metal using Contiv.
### Automated Deployment
Contiv will be deployed as a component of the overall envrironment configuration. Each system deployment's environemnt is specific to a customer and/or application requirements. Therefor, a method is provided for choosing a networking option in tbe environment build autoamtion. 

The users will define the choice of networking fabric in one step by setting an environment variable in the "tricorder-environments" file.

1. Set environment variable in envrionments ".cfg" file as below:

	`NETWORKING_OPTION=Contiv`  
	
	  [Example Configuration File](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-environments/browse/prod/vsphere/tenants/example/example.cfg)

### Manual Deployment 

Contiv can be deployed manually through the following steps:  
1.	Assumes Docker and Kubenetes have been deployed

2. Initialize Kubernetes with IP for Contiv
	
	`kubeadm init --service-cidr 10.254.0.0/16`
3. Download Contiv package
        
        curl -L -O https://github.com/contiv/install/releases/download/1.0.0-beta.4/contiv-1.0.0-beta.4.tgz
	    tar oxf contiv-1.0.0-beta.4.tgz 
	    cd contiv-1.0.0-beta4
4. Install and Run Contiv using as parameter the private IP of the machine on which us running
    
    `./install/k8s/install.sh -n <MACHINE_PRIVATE_IP>`
5. Provide minimal configuration for Contiv, which includes the overlay network address
	
	`netctl net create -t default --subnet=20.1.1.0/24 default-net`
	
## Additional information

Please refer to the below deployment guide for:

1. *Detailed deployment instructions* 
2. *Steps to verify networking deployment*
 
	[Networking Deployment Guide](https://cisco.jiveon.com/docs/DOC-1702651)

For Contiv support, please join the below Slack channel:

[Contiv Slack Channel](https://contiv.slack.com)

{% include links.html %}
