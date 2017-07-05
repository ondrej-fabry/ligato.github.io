---
title: Infrastructure Services
tags: [infrastructure]
keywords:
sidebar: mydoc_sidebar
permalink: mydoc_infrastructure.html
folder: mydoc
---

## Tricorder Infrastructure Services

Community deploys the following support components in Kubernetes to be used by application services:


|  Component | Version  | Image Build | Deployment Role
|  --------- | -----------  |  ---- |
|  Cassandra | 3.10 | [Cassandra image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/cassandra)  | [Cassandra deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/cassandra)
|  Elk  | 5.1.2 | [elk images](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/elasticsearch) [doc](http://elk-docker.readthedocs.io/) | [elk deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/elasticsearch)
|  Etcd | 3.1.3 | [etcd image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/etcd) | [etcd deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/etcd)
|  Fluentd | 1.1 | [fluentd image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/fluentd) | [fluentd deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/fluentd)
|  Kafka | 0.10.1.1 | [kafka image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/kafka)  | [kafka deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/kafka)
|  Zookeeper | 3.4.9 |  [zookeeper image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/zookeeper) | [zoopeeper deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/kafka/templates)
|  Redis | 2.8.19 | [redis image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/redis) | [redis deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/redis)
|  VPP | 17.04 | [vpp image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/tric_vpp_vnf_agent)  | [vpp deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/tric_vpp_vnf_agent)
|  Zipkin | 1.20 | [zipkin image](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse/zipkin) | [zipkin deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/zipkin)
| Kubernetes | 1.5.4 |  | [kubernetes deployment](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-bootstrap/browse/ansible/roles/kubernetes) 

\\
Other support images are available in bitbucket : [tricorder-base-images](https://bitbucket-eng-sjc1.cisco.com/bitbucket/projects/TRIC/repos/tricorder-baseimages/browse)


{% include links.html %}
