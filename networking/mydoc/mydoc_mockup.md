---
title: Tricorder UI mockup
tags: [ui]
keywords:
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_mockup.html
folder: mydoc
---

## Deployer Mockup Overview
Deployer mockup shows a layer of UI to be integrated with Spinnaker. It is API driven (API first) the goals is to add a Cisco look and feel and include addional features that are important for clous native deployment. \\
The mockup is developed using Axure.

## Axhare Link
<http://yvsyxk.axshare.com/log_in.html>  (pwd to access the mockup is tricorder)


## Axure code:
<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-ui-mockup/browse>


## Connection to Spinnaker (Optional):

It is not need, but if you want show the real api page (it is controlled in the UI -> settings), you need connection to Spinnaker: \\
SSH to Spinnaker to create Proxy: \\
`ssh -A -L 9000:localhost:9000 -L 8084:localhost:8084 -L 8087:localhost:8087 -i <jusilvei-keypair.pem>  ubuntu@Spinnaker_IP `

After ssh you can access the Spinnaker console and mockep will also be able to access:\\
<http://localhost:9000/> \\

Spinnaker API:\\
<http://localhost:8084/swagger-ui.html>


{% include links.html %}
