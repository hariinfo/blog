---
layout: post
title: Spring boot and Consul in Kubernetes
date: 2018-04-15
categories: Open Source
---

## Synopsis
---
In this post, I will walk through an example to demonstrate the usage of
Spring boot and Consul in the Kubernetes environment

To keep things simple, I will not make use of TLS and tokens for communication between
Consul Agent and Server.

The logical view for this setup is quite simple and can be tested both in Minikube
and Kubernetes on the Cloud.

Code example for this blog is available here.
[https://github.com/hariinfo/service-c](https://github.com/hariinfo/service-c)

## Logical view
---

```
------- POD 1 --------------------    ------- POD 2 ----------------------
                                 |   |   Consul server 0 (Leader)         |
                                 |    ------------------------------------
                                 |    --------- POD 3 --------------------  
Spring Boot App -> Consul Agent  | ->|   Consul server 1                  |
(container 1)      (container 2) |    ------------------------------------
                                 |    -------- POD 4 ---------------------       
                                 |   |   Consul server 2                  |
----------------------------------    ------------------------------------
```

## Deploy microservice and consul agent
```
kubectl create -f deploy.yaml    
```

## Create consul configmap
```
kubectl create -f consul-config.yaml
```

## Create consul service
```
kubectl create -f consul-server-service.yaml
```

## Create consul deployment
```
kubectl create -f consul-server-deploy.yaml
```

## Validate Consul Agent and Server Logs
Validate the logs to ensure consul server and client have joined successfully
through the gossip protocol
```
kubectl logs consul-0
kubectl logs consul-1
kubectl logs consul-2
```

## Accessing the web UI
```
kubectl port-forward consul-0 8500:8500
```
Visit http://127.0.0.1:8500 in your web browser.

## Test your application
From the consul UI, create a simple key value pair
Key=config/consulAppSample/servicec
Value=Hello World!!!

Now access the Spring boot URL, In minikube setup you may retrieve the service url
as follows

```
minikube service service-c --url
```
Visit [http://<SpringBootURL>/getKV](http://<Spring Boot URL>/getKV) in your web browser.
You should now see the KV value that you entered from Consul UI



## Cleanup and Purge command
```
kubectl delete deployment,service service-c
kubectl delete statefulset, service consul
kubectl delete config consul-config

```
