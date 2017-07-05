---
title: Zing
tags: [Zing]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_lessons_zing.html
folder: mydoc
---


# Zing

From Chase Wolfinger (cwolfing) <cwolfing@cisco.com> 

In the MCBU, we have a number of mission critical control applications where we need to maintain sub 20ms SLA(s) for our customers.  
If your application stack has Java (either as a Cassandra DB or as an app itself) then you will most likely run into the pains 
( or joys … ) of  tuning Java garbage collection.   We recently switched to use Azul Zing’s JVM for the pause-less garbage collector 
that they provide and saw the following improvement with one app (improved response time and reduced jitter):

{% include image.html file="Zing.png" alt="Zing" caption="Zing" %}

I would suggest if you have a Java app or a Cassandra DB that requires strict SLA(s) then please reach out to me and I can 
provide more detail on our experience with Zing
 

{% include links.html %}
