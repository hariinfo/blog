---
layout: post
title: Network Audit Tool
date: 2019-01-10
---

## Synopsis
I recently completed setup of [raspberry pi](https://www.raspberrypi.org/) file server in my home network. In this process, I had to open up couple of ports to access my file server from public domain. I was actively looking for an open source network scanner to ensure I did not create vulanarability within my home network by opening up these ports to the internet.
In this blog, I would like to talk about one such tool that comes in pretty handy to perform a network scan to identify network security vulnerabilities.


## Introduction
nmap [https://nmap.org/](https://nmap.org/) is a pretty popular tool on the unix platforms there is also a gui version of this called Zenmap [https://nmap.org/zenmap/](https://nmap.org/zenmap/)
I will walk you through the setup and scanning of my raspberry pi server in this blog.

## Setup
This is pretty startight forward, download the binaries from this location
[https://nmap.org/download.html](https://nmap.org/download.html)
and follow the details based on your target platform.

## Configuration and Scanner 
After successfull installation you may enter the ip address of any host within your network. In the screenshot below, I have entered the ip address of my PI
![Zenmap Scanner](/assets/zenmap1.png)

Click on the ports/hosts tab, you should be able to see a report indicating open ports and few additional details around what is runnin on those ports

![Zenmap Scanner](/assets/zenmap2.png)
