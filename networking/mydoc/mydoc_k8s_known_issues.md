---
title: Kubernetes known issues
tags: [kubernetes]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_k8s_known_issues.html
folder: mydoc
---

## Slow Kubernetes 

We are seeing that it is common to get K8s dashboard unresponsive after a few days. \\
The slow response is caused by high disk read I/O and when there is 0 swap space the dashboard process will have I/O wait problem (spinning in the UI dashboard). \\
Here are the steps to create the swap space.

*	swapon –s and free –m # Check make sure the swap space is 0 before proceeding
*	dd if=/dev/zero of=/dtswap count=8192 bs=1MiB
*	chmod 600 /dtswap
*	mkswap /dtswap
*	swapon /dtswap

Note: After completing the steps run the “top” command to verify there are 8 GB of swap space


## Old K8s Replica Sets not getting deleted

Kubernetes documentation (<https://kubernetes.io/docs/concepts/workloads/controllers/deployment>) says that "By default, two previous Deployment’s rollout history are kept in the system so that you can rollback anytime you want” , but it is not working properly on k8s 1.5.4.

We need to enforce revison history :
Add revisionHistoryLimit: 1 or 2 in the deployment yaml, like: 

    spec:
      replicas: 1
      revisionHistoryLimit: 1
      selector:
        matchLabels:
          name: tric-sample-go-service


See the full yaml at <https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/templates/tric_sample_java_service.j2>



{% include links.html %}
