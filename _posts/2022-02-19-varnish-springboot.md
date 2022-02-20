---
title: Caching. Caching. Caching....
updated: 2022-01-03 10:38
---

## Synopsis
A significant element of optimization of site performance is achieved through caching. For best results, caching should be applied across multiple tiers of the application architecture. We are focused explicitly on sever side edge cache in this article. First, let us understand a few concepts related to edge cache. A request to the server ideally has the following path (I have eliminated components such as DNS servers and load balancers for simplicity)

![Caching Logical View](images/caching.svg)


* **CDN** - These are ideally providers such as Akamai, Fastly who run an extensive network of caching servers. The servers are located close to the user location. For example, if you are browsing from the Chicago area, your traffic would reach a CDN server in the Chicago metro region, reducing the overall cost associated with the network.

* **Origin front door cache** - In case of a cache miss at the CDN layer. i.e., CDN did not see a previously cached copy of the data or expired cached entry. CDN will forward the request to the origin server in your data center. You can optionally use another layer of cached server using technologies such as [Varnish](https://varnish-cache.org/) or [Memcached](https://memcached.org/) to reduce load to the backed microservice or applications.

* **Origin Server** - If the Origin cache server is unable to find a cached entry, it will forward the request to its origin server, which could be a backend application or a microservice that serves the data using an API (such as REST).

* **DB Cache Layer** - We cacn further Optimize the performance of Orgin server by caching the database results using a DB cache layer, this would avoid us to make frequent calls to database thereby reducing load on the database. [Redis](https://redis.io/) is a popular tool that can be used for server side caching.

With this background, let us dig into the **Origin edge case** layer, and illustrate this for a sample Spring boot application that emits certain JSON response for REST end points. We will leverage Varnish as our front end caching layer for the microservice.

## The Setup

...|Varnish Cache| -> |Spring boot microservice| -> ...

Complete sample code for this example is availabe in this [Git repository](https://github.com/hariinfo/spring-learn/tree/main/varnish-spring)

