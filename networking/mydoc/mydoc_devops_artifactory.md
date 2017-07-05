---
title: Devops Artifactory
tags: [devops]
keywords: devops
last_updated: March 26, 2017
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_devops_artifactory.html
folder: mydoc
---

## Internal Repo:
vnf-docker-dev-local (also mirrored)\\
vnf-docker-devops-local
 
## Credential and Configuration Auth:
Username and Password is available in box
 
## Basic Usage:
 
1. Initial Setup
* Requires a Docker client running version 1.6 or newer
* Enter credentials for the deployer user:
docker login –u username -p password -e <email>@cisco.com dockerhub.cisco.com
 
2. Deploy
* docker pull hello-world
* docker tag hello-world dockerhub.cisco.com/vnf-docker-dev/hello-world
* docker push dockerhub.cisco.com/vnf-docker-dev/hello-world
 
3. Download
* docker pull dockerhub.cisco.com/vnf-docker-dev/hello-world
 
* Use your cec account to login to Artifactory
* <http://engci-maven-master.cisco.com/artifactory/list/vnf-docker-devops/>
* <http://engci-maven-master.cisco.com/artifactory/list/vnf-docker-dev/>
 
### External Repo:
vnf-docker-dev-local
 
External repo url and instructions to get the password,
1. <https://devhub.cisco.com/artifactory>
2. Click SSO link at the login 
3. Use cec account to login
4. Get the API key by clicking the username at top right corner
5. Click view in the user profile page to see the API key
6. Use the api key with curl command or use the infos below to pull the artifacts from DMZ
 
Note: The DMZ repo (external) is a read only mirror setup on the internal repo. You have to first deploy images to the internal repository and then the image will be available in the external repo.
 
### Connecting to devhub-docker.cisco.com
docker login -u <user> -p <API key used for DMZ access> devhub-docker.cisco.com
 
### Download:
docker pull devhub-docker.cisco.com/vnf-docker-dev/hello-world

{% include links.html %}
