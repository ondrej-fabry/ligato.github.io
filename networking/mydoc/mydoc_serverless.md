---
title: Serverless
tags: [governance]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_serverless.html
folder: mydoc
---


# Serverless Articles That are Good 

**Martin Fowler**  

Link is:   https://martinfowler.com/articles/serverless.html

Key Points: 

* referred to in this article as FaaS, “Function as a Service”. 
* “server-side logic … run in stateless compute containers that are event-triggered, ephemeral (may only last for one invocation), and fully managed by a 3rd party.”
* You don’t have to manage the infrastructure at all - just upload your code and it runs and scales automatically
* JVM-based languages may not be the best choice for serverless, because there is overhead in spinning up a JVM.  Serverless processes start/stop as needed, so could you pay this overhead repeatedly.
* You have to work within the provider’s ecosystem of services and restrictions -- approved languages, supported service integrations, etc.  There are some open source efforts to make vendor-neutral serverless frameworks so you can port your function from vendor to vendor, but it’s not clear how mature these are.

# Discussion Forum

Please subscribe to serverless-core-team@cisco.com and/or serverless-interest 

#  Cisco Jive site 

A page for “Serverless And Function As A Service” computing; This community space is dedicated to discuss/suggest/track any progress on this subject. We need your help and support to get most out of this initiative, so that we can create right strategy for Cisco (within CPSG group) and plan right to invest on this technology. Please check out the link below
 
       https://cisco.jiveon.com/docs/DOC-1728970
 

{% include links.html %}
