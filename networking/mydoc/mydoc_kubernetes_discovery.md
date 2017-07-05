---
title: Kubernetes - Discovering Services, Pods, and Containers
tags: [kubernetes, discovery, baseclass, tricorder, code]
keywords:
summary: ""
sidebar: mydoc_sidebar
permalink: mydoc_kubernetes_discovery.html
folder: mydoc
---

Microservice in a Kubernetes cluster must discover the endpoint it wishes to invoke because containers and pods may get relocated to any node at runtime.  This article discusses how to conduct such discovery from within a microservice container using Kubernetes API.

## Accessing Kubernetes API from within a Container

**Inside a container**, you can find environment variables about Kubernetes master, such as

```
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT_HTTPS=443
```

Construct Kubernetes API endpoint URL using these variables:

```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api
```

Obtain API token from the file

```
/var/run/secrets/kubernetes.io/serviceaccount/token
```

An API call using curl command is as simple as follows:

```
$ KUBE_TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
$ curl -k -X GET -H "Authorization: Bearer ${KUBE_TOKEN}" https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/
```

Sample response as follows

```
{
  "kind": "APIVersions",
  "versions": [
    "v1"
  ],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "172.22.16.63:6443"
    }
  ]
}
```

## Discovering Services

API endpoints

```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/v1/services
```
```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/v1/namespaces/<namespace>/services
```
```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/v1/namespaces/<namespace>/services/<service_name>
```

The response is service record(s) in a JSON document.  See excerpt from a response below:

```
{
  "kind": "ServiceList",
  "apiVersion": "v1",
  "metadata": {
    "selfLink": "/api/v1/services",
    "resourceVersion": "393310"
  },
  "items": [
    {
      "metadata": {
        "name": "tric-sample-service",
        "namespace": "default",
        "selfLink": "/api/v1/namespaces/default/services/tric-sample-service",
        "uid": "7fdf4cbe-0da5-11e7-966a-005056915c49",
        "resourceVersion": "1572",
        "creationTimestamp": "2017-03-20T19:43:32Z",
        "labels": {
          "app": "tric-sample-service",
          "tier": "backend"
        },
        "annotations": {
          "kubectl.kubernetes.io/last-applied-configuration": "{\"kind\":\"Service\",\"apiVersion\":\"v1\",\"metadata\":{\"name\":\"tric-sample-service\",\"creationTimestamp\":null,\"labels\":{\"app\":\"tric-sample-service\",\"tier\":\"backend\"}},\"spec\":{\"ports\":[{\"port\":8000,\"targetPort\":6509}],\"selector\":{\"tier\":\"backend\"},\"type\":\"NodePort\"},\"status\":{\"loadBalancer\":{}}}"
        }
      },
      "spec": {
        "ports": [
          {
            "protocol": "TCP",
            "port": 8000,
            "targetPort": 6509,
            "nodePort": 31421
          }
        ],
        "selector": {
          "tier": "backend"
        },
        "clusterIP": "10.101.204.105",
        "type": "NodePort",
        "sessionAffinity": "None"
      },
      "status": {
        "loadBalancer": {}
      }
    },

    ...

  ]
}
```

**NOTE**: Services in Kubernetes consistently maintain a well-defined endpoint for pods. These endpoints remain the same, even when the pods are relocated to other nodes or when they get restarted.  These static endpoints usually can be resolved by KubDNS.  An API call may not be necessary, depending on what you need from the service.

## Discovering Pods and Containers

API endpoints

```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/v1/pods
```
```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/v1/namespaces/<namespace>/pods
```
```
https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT_HTTPS}/api/v1/namespaces/<namespace>/pods/<pod_name>
```

The response is pod record(s) in a JSON document.  See excerpt from a response below:

```
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {
    "selfLink": "/api/v1/pods",
    "resourceVersion": "395260"
  },
  "items": [
    {
      "metadata": {
        "name": "tric-sample-service-990095713-7n7xl",
        "generateName": "tric-sample-service-990095713-",
        "namespace": "default",
        "selfLink": "/api/v1/namespaces/default/pods/tric-sample-service-990095713-7n7xl",
        "uid": "7fe4f471-0da5-11e7-966a-005056915c49",
        "resourceVersion": "1650",
        "creationTimestamp": "2017-03-20T19:43:32Z",
        "labels": {
          "app": "tric-sample-service",
          "pod-template-hash": "990095713",
          "tier": "backend"
        },
        "annotations": {
          "kubernetes.io/created-by": "{\"kind\":\"SerializedReference\",\"apiVersion\":\"v1\",\"reference\":{\"kind\":\"ReplicaSet\",\"namespace\":\"default\",\"name\":\"tric-sample-service-990095713\",\"uid\":\"7fe4765a-0da5-11e7-966a-005056915c49\",\"apiVersion\":\"extensions\",\"resourceVersion\":\"1575\"}}\n"
        },
        "ownerReferences": [
          {
            "apiVersion": "extensions/v1beta1",
            "kind": "ReplicaSet",
            "name": "tric-sample-service-990095713",
            "uid": "7fe4765a-0da5-11e7-966a-005056915c49",
            "controller": true
          }
        ]
      },
      "spec": {
        "volumes": [
          {
            "name": "default-token-kn932",
            "secret": {
              "secretName": "default-token-kn932",
              "defaultMode": 420
            }
          }
        ],
        "containers": [
          {
            "name": "tric-sample-service",
            "image": "devhub-docker.cisco.com/vnf-docker-dev/tric_sample_service:1.0.0.83.9a173b03",
            "ports": [
              {
                "name": "p1",
                "containerPort": 6509,
                "protocol": "TCP"
              },
              {
                "name": "p2",
                "containerPort": 6543,
                "protocol": "TCP"
              },
              {
                "containerPort": 8000,
                "protocol": "TCP"
              }
            ],
            "resources": {
              "requests": {
                "cpu": "10m",
                "memory": "1200Mi"
              }
            },
            "volumeMounts": [
              {
                "name": "default-token-kn932",
                "readOnly": true,
                "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount"
              }
            ],
            "terminationMessagePath": "/dev/termination-log",
            "imagePullPolicy": "Always"
          }
        ],
        "restartPolicy": "Always",
        "terminationGracePeriodSeconds": 30,
        "dnsPolicy": "ClusterFirst",
        "serviceAccountName": "default",
        "serviceAccount": "default",
        "nodeName": "devtricorder2-minion-01",
        "securityContext": {},
        "imagePullSecrets": [
          {
            "name": "tricregkey"
          }
        ]
      },
      "status": {
        "phase": "Running",
        "conditions": [
          {
            "type": "Initialized",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2017-03-20T19:43:32Z"
          },
          {
            "type": "Ready",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2017-03-20T19:43:43Z"
          },
          {
            "type": "PodScheduled",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2017-03-20T19:43:32Z"
          }
        ],
        "hostIP": "172.22.16.64",
        "podIP": "10.44.0.10",
        "startTime": "2017-03-20T19:43:32Z",
        "containerStatuses": [
          {
            "name": "tric-sample-service",
            "state": {
              "running": {
                "startedAt": "2017-03-20T19:43:42Z"
              }
            },
            "lastState": {},
            "ready": true,
            "restartCount": 0,
            "image": "devhub-docker.cisco.com/vnf-docker-dev/tric_sample_service:1.0.0.83.9a173b03",
            "imageID": "docker-pullable://devhub-docker.cisco.com/vnf-docker-dev/tric_sample_service@sha256:678f0f34c94ff93e7daebac37edeb954df55237702228da67c203e6c9570c5e8",
            "containerID": "docker://5da30d2750e5c8401da4cd0b7e43eb67742ff29128ff5265f768a2af2e24ab22"
          }
        ]
      }
    },
  ]
}
```
For example, if you need to make a direct REST call to a container (and security settings allows), you may extract the value of "podIP" plus the value of "containerPort" of that container that supports TCP protocol from the JSON document.  Construct the URL by using these two pieces of information with proper protocol, either http or https depending on the protocol supported by the container on the given port.  Like,

```
https://<podIP>:<containerPort>
```

## Discovering other Kubernetes Resources

You can use Kubernetes API to discover other resources, such as nodes, deployments, daemonsets, replicasets...etc.  Refer to [Kubernetes documents](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) for more details.

{% include links.html %}
