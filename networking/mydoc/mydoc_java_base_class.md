---
title: Java Base Classes
tags: [baseclass, code]
keywords:
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_java_base_class.html
folder: mydoc
---

## Overview

This document covers the available java base classes.


## Support

Tricorder uses CiscoSpark for real time support and collaboration. There is a team space on spark here: <https://web.ciscospark.com/rooms/4a4a4930-3c1d-11e7-a30a-89723d139f6b/chat>

E-mail sp-cloud-native-admins@cisco.com with your user ID requesting admission to the spark room


## Java Base Classes & Support Microservices

Java Base Class - Logging and Circuit Breaker:\\
&nbsp;&nbsp;&nbsp;&nbsp;* Common logging API, message format, and configuration methods\\
&nbsp;&nbsp;&nbsp;&nbsp;* Option to wrap service request using Hystrix Command to execute potentially risky functionality\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-base-java/browse>

Java Base Class - Cache (Redis):\\
&nbsp;&nbsp;&nbsp;&nbsp;API to cache complex data using external cache service\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-cache-java/browse>

Java Base Class - Messaging (Kafka):\\
&nbsp;&nbsp;&nbsp;&nbsp;API to publish and subscribe for messages to/from Kafka message bus\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-cache-java/browse>

Java Base Class - Tracing (Spring Cloud Sleuth):\\
&nbsp;&nbsp;&nbsp;&nbsp;API to trace service requests (Span) across (micro-)services\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-tracing-java/browse>

Java Base Class - Database (Kundera JPA):\\
&nbsp;&nbsp;&nbsp;&nbsp;Data access using Kundera and Java Persistence API's\\
&nbsp;&nbsp;&nbsp;&nbsp;- Contribution of the Sereno team\\
&nbsp;&nbsp;&nbsp;&nbsp;- Current implementation supports Cassandra\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-database-java/browse>

Java Support Microservice - CSRF (Cross Site Request Forgery):\\
&nbsp;&nbsp;&nbsp;&nbsp;A service to allocate and validate authentication token for added security to service requests\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-csrf-service/browse>


## Java Sample Microservice

Java Sample Microservice includes build instructions:\\
&nbsp;&nbsp;&nbsp;&nbsp;<https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-sample-service-java/browse>




{% include links.html %}
