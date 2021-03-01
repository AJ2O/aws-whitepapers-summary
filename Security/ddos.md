# **AWS Best Practices for DDoS Resiliency**

# Sections
- [**AWS Best Practices for DDoS Resiliency**](#aws-best-practices-for-ddos-resiliency)
- [Sections](#sections)
- [Overview](#overview)
  - [Prerequisites](#prerequisites)
- [Introduction: Denial of Service Attacks](#introduction-denial-of-service-attacks)
  - [Definition](#definition)
  - [Infrastructure Layer Attacks](#infrastructure-layer-attacks)
    - [UDP Reflection Attacks](#udp-reflection-attacks)
- [Mitigation Techniques](#mitigation-techniques)
- [Attack Surface Reduction](#attack-surface-reduction)
- [Operational Techniques](#operational-techniques)
- [Conclusion](#conclusion)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)

This summary is based off of the December 2019 revision of the **AWS Best Practices for DDoS Resiliency** whitepaper. It provides the reader with guidance to improve the DDoS resiliency of their apps running on AWS. This is done through outlining the practices that fit into a DDoS mitigation strategy, and the corresponding AWS services and features that can help a customer replicate those practices to protect their apps.

## Prerequisites
- Basic knowledge of the [OSI Model](https://www.cloudflare.com/en-gb/learning/ddos/glossary/open-systems-interconnection-model-osi/)

# Introduction: Denial of Service Attacks

## Definition
A **Denial of Service (DoS)** attack is a deliberate attempt to make an application or website unavailable to its users, such as flooding and overwhelming it with network traffic. A **Distributed Denial of Service (DDoS)** attack uses multiple sources (infected computers, routers, IoT devices, etc.) to orchestrate a DoS attack against a target. The diagram below illustrates a simple DDoS attack.

![DDoS](../Diagrams/DDoS.png)

DDoS attacks are most common at the layers 3, 4, 6, and 7 of the [OSI Model](https://www.cloudflare.com/en-gb/learning/ddos/glossary/open-systems-interconnection-model-osi/), and the chart below displays examples of attacks in each layer. These attack types will be discussed in detail in the following sections.

<html>
<table>
  <tr>
    <th>Layer</th>
    <th>Data Unit</th>
    <th>Description</th>
    <th>Attack Vector Examples</th>
  </tr>
  <tr>
    <th>7 - Application</th>
    <td>Data</td>
    <td>Network access to application</td>
    <td>HTTP floods, DNS query floods</td>
  </tr>
  <tr>
    <th>6 - Presentation</th>
    <td>Data</td>
    <td>Data representation and encryption</td>
    <td>TLS Abuse</td>
  </tr>
  <tr>
    <th>4 - Transport</th>
    <td>Segments</td>
    <td>End-to-end connections and reliability</td>
    <td>SYN floods</td>
  </tr>
  <tr>
    <th>3 - Network</th>
    <td>Packets</td>
    <td>Path determination and logical addressing</td>
    <td>UDP reflection attacks</td>
  </tr>
</table>
</html>

## Infrastructure Layer Attacks
This document will refer to Layers 3 and 4 as the *infrastructure layer*, and it is the most commonly attacked layer. Large volumes of network traffic can overwhelm the network capacity, tie up firewall and server systems.

### UDP Reflection Attacks

# Mitigation Techniques

# Attack Surface Reduction

# Operational Techniques

# Conclusion

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
