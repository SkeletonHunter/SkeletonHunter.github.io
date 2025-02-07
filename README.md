![deployment](https://img.shields.io/github/deployments/SkeletonHunter/SkeletonHunter.github.io/github-pages?label=deployment) ![Platform](https://img.shields.io/badge/platform-Linux-8A2BE2) ![license](https://img.shields.io/github/license/SkeletonHunter/SkeletonHunter.github.io)

## 0. Table of Contents

* [Introduction](#1-introduction)
* [Code](#2-code)
* [Data](#3-data)

## 1. Introduction

The flexibility, portability, and isolation characteristics have made containers a popular environment for large model training in recent years. Unfortunately, these advantages render the network support for containerized large model training extremely challenging, due to the high dynamics of containers, the complex interplay between underlay and overlay networks, and the stringent requirements on failure detection and localization. Existing data center network debugging tools, which rely on comprehensive or opportunistic monitoring, are either inefficient or inaccurate in this setting.

This paper presents SkeletonHunter, a container network monitoring and diagnosis system that leverages the intrinsic and regular sparsity of the network traffic incurred by large model training. Its key idea is to reason about the traffic skeleton, which comprises a crucial set of network paths consistently traversed by the training traffic, so as to reliably detect and localize network failures in short time. We deployed it in production for six months, uncovering 4,816 network failures with 98.2% precision and 99.3% recall, and localizing them with a high accuracy of 95.7%. After fixing 98% problematic network components, the monthly network failure rate has significantly dropped by 99.1%.

## 2. Code

The code for release is currently undergoing the internal review. Once the review process is completed, we will make the corresponding content publicly available.

The code is organized as follows:

<table>
  <tr>
    <td>Directory</td>
    <td>Description</td>
    <td>Source Code</td>
  </tr>
  <tr>
    <td rowspan='2'>common</td>
    <td rowspan='2'>common dependencies like host utils</td>
    <td>host.go</td>
  </tr>
  <tr>
    <td >pci.go</td>
  </tr>
  <tr>
    <td rowspan='2'>config</td>
    <td rowspan='2'>congurations that store task/pod info</td>
    <td>global.go</td>
  </tr>
  <tr>
    <td>task.go</td>
  </tr>
  <tr>
    <td rowspan='3'>coretask</td>
    <td rowspan='3'>probing task implementations</td>
    <td>rdmapingcore.go</td>
  </tr>
  <tr>
    <td>taskgenerator.go</td>
  </tr>
  <tr>
    <td>taskscheduler.go</td>
  </tr>
  <tr>
    <td rowspan='2'>daemon</td>
    <td rowspan='2'>daemon services to keep the process alive</td>
    <td>agent_ctl.sh</td>
  </tr>
  <tr>
    <td >agent_demon.sh</td>
  </tr>
  <tr>
    <td rowspan='1'>main</td>
    <td rowspan='1'>entry point of the agent</td>
    <td>main.go</td>
  </tr>
  <tr>
    <td rowspan='2'>taskmessage</td>
    <td rowspan='2'>helper functions for communicating with the controller</td>
    <td>messager.go</td>
  </tr>
  <tr>
    <td>msgkey.go</td>
  </tr>
</table>

## 3. Data

We provide data samples [here](https://github.com/SkeletonHunter/SkeletonHunter.github.io/blob/main/data/samples.csv). Note that some fields are hashed for privacy and security reasons.

Below are the description for some of the data fields:

<table>
  <tr>
    <td>Field Name</td>
    <td>Description</td>
  </tr>
  <tr>
    <td>dial</td>
    <td>a flag that indicates whether the connectivity is successful</td>
  </tr>
  <tr>
    <td>dst_ip</td>
    <td>IP address of the pinged container</td>
  </tr>
  <tr>
    <td>dst_node_id</td>
    <td>ID of the pinged container in a training cluster</td>
  </tr>
  <tr>
    <td>dst_node_sn</td>
    <td>SN of the host that runs the pinged container</td>
  </tr>
  <tr>
    <td >dst_port</td>
    <td>port number for processing ping packets</td>
  </tr>
  <tr>
    <td>dst_real_ip</td>
    <td>IP of the host that runs the pinged container</td>
  </tr>
  <tr>
    <td>tenant_id</td>
    <td>tenant ID of the training task</td>
  </tr>
  <tr>
    <td>latency</td>
    <td>end-to-end latency (RTT) in microsecond</td>
  </tr>
  <tr>
    <td>loss_rate</td>
    <td>end-to-end packet loss rate</td>
  </tr>
  <tr>
    <td>src_ip</td>
    <td>IP address of the container that initiates the probing</td>
  </tr>
  <tr>
    <td>src_node_id</td>
    <td>ID of the container that initiates the probing</td>
  </tr>
  <tr>
    <td>src_node_sn</td>
    <td>SN of the host running the container that initiates the probing</td>
  </tr>
  <tr>
    <td >src_port</td>
    <td>port number for processing ping packets</td>
  </tr>
  <tr>
    <td>src_real_ip</td>
    <td>IP of the host running the container that initiates the probing</td>
  </tr>
  <tr>
    <td >ping_time</td>
    <td >timestamp of the ping data</td>
  </tr>
  <tr>
    <td>task_type</td>
    <td>enum fields for RDMA/TCP/Traceroute pings</td>
  </tr>
</table>
