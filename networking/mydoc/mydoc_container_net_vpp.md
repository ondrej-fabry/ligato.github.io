---
title: Container Networking - VPP
tags: [networking]
keywords:
summary: "Vector Packet Processing (VPP) is an FD.io open source project to deiver a software-based and high-performancepacket processing networking solution. The design of VPP is hardware, kernel, and deployment (bare metal, VM, container) agnostic.  It runs completely in userspace. An effort is under way at Cisco to integrate VPP with Contiv. This integration would deliver a high performance networking solution along with policy management for container based Virtual Network Functions (VNFs)."
sidebar: mydoc_sidebar
permalink: mydoc_container_net_vpp.html
folder: mydoc
layout: network
---

## Background

### VNF Container Networking Platform
The *VPP VSwitch*, *VNF Agent*, *VNF Agent Plugin* and *Contiv integration* will collectively be utilized as a platform to deliver data plane container networking and policy management for VNF prodcuts such as Cloud CMTS and others. 

### VPP VSwitch

VPP is an extensible framework that provides out-of-the-box production quality switch/router functionality in a Linux user space application. It is the open source version of Cisco's Vector Packet Processing (VPP) technology: a high performance, packet-processing stack that can run on commodity CPUs. The benefits of this implementation of VPP are its high performance, its modularity, and running on any standard hardware. In addition, the same VPP image runs in any hosts, as a VM guest or a container. VPP is built on a modular architecture which enables it to run as a Container or a Virtual Network Funtion. To start, the most relevant application of VPP in our case are data plane container networking for Cisco VNFs.

{% include image.html file="Networking-VPP.png" caption="**VPP Overview**"%}

VPP VSwitch is described in the link below. 

[VPP VSwitch Detailes (FD.io)](https://fd.io/technology)


The other compoenents comprising the VNF container networking and policy management are VNF Agent, Agent Plugins and Contiv integration. These components are further described with their associated links in the following sections. Contiv integration is currently work-in-progress and the detailed descrption along with relevant links will be included as they become available.   

### VNF Agent

VNF Agent is developed by Cisco as a base component to enable management (control plane) for container-based VNFs. The VNF Agent is comprised of two main componets:

* **Plugin Framework:** provides plugin lifecycle management
* **Agent Core:** provides connectivity to common services, VPP VSwitch and access to ervices through REST APIs

### VNF Agent Plugin

VNF Agent Plugin provides VNF specific networking or management functionality which can be loaded onto the base VNF Agent. The plugins are implemented by VNF teams to provide specific VNF functionality and/or provide management for specialized VNF plugins which are loaded into VPP. 

Further detailes on VNF Agent, VNF Agent Plugin components and their  architecture are in the below link:

[VNF Agent Repository Page](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-vpp-agent/browse)

### Orchestration and Policies 

The below digaram depicts the functional view of the integration amongst base VPP (VSwitch), VNF Agents, VNF Agent Plugin and Contiv (Netmaster & Netplugin). In this digaram, the orchestration and policy management is initiated from an external controller and then  flows through Contiv Netmaster to Contive Netplugin and evertually down to the VNFs or directly to the VPP vSwitch. 

{% include image.html file="Networking-VPP-Orchestration.png" caption="**VNF Data Plane Policy Orchestration with VPP, VNF Agent and Contiv integration**"%}

### Service Function Chaining 

The below diagram depicts an example scenario for a Service Function Chain (SFC). In this example, the VNF containers providing Service Functions 1, 2 and 3 are connected across two servers requiring the presence of the VPP VSwitches and an Overaly tunnel for connectivity across the two servers. 

{% include image.html file="Networking-VPP-SFC-Example.png" caption="**Data Plane Service Function Chaining with Contiv and VPP**"%}



## Deployment
The Cloud Native platform enables automated deployment of the networking along with the environment and application containers. The below sections describe the automated and manual deployement options. Users should take advantage of the automated deployment to speed up the deployment and to validate connectivity and policy enforcement. The following sections describe the required deployment steps for AWS, WMWare and Bare Metal.
### Automated Deployment
VPP will be deployed as a component of the overall envrironment configuration. Each system deployment's environemnt is specific to a customer and/or application requirements. Therefor, a method is provided for choosing a networking option in tbe environment build autoamtion. 

The users will define the choice of data plane switching fabric in one step by setting an environment variable in a configuraiton file.

Set environment variable in envrionments ".cfg" file as below:

`VPP_NIC_IP=<Insert IP address of IPV4_ADDRESS_MASTER_NIC1>`
 
[Example Configuration File](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-environments/browse/prod/vsphere/tenants/example/example.cfg)
	
For more detailes on VPP and VPP Agent build and deployment implementation, please refer to the below project respository links:

   [VPP Build and Deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-vpp/browse)
   
   [VNF Agent Build and Deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-vpp-agent/browse)   
   
### Manual Deployment 

VPP can be deployed manually by following the instructions here: 
 
 [VPP Manual Build and Deployment Instructions](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-vpp/browse)

	
## Additional information

Please refer to the below Networking Deployment guide for:

1. *Detailed deployment instructions*
2. *Steps to verify networking deployment*

	[Networking Deployment Guide](https://cisco.jiveon.com/docs/DOC-1702651)

Additional details on Contiv networking and policy management here:	

[Contiv Container networking and policy management](http://contiv.github.io/)


[//]: # "Comment for image sizing: alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>"


{% include links.html %}
