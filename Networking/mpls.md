# **Integrating AWS with Multiprotocol Label Switching**

# Sections
- [**Integrating AWS with Multiprotocol Label Switching**](#integrating-aws-with-multiprotocol-label-switching)
- [Sections](#sections)
- [Overview](#overview)
- [Introduction](#introduction)
- [Overview of AWS Networking Services and Core Technologies](#overview-of-aws-networking-services-and-core-technologies)
  - [Amazon Virtual Private Cloud](#amazon-virtual-private-cloud)
  - [AWS Direct Connect and VPN](#aws-direct-connect-and-vpn)
  - [Internet Gateway](#internet-gateway)
  - [Customer Gateway](#customer-gateway)
  - [Virtual Private Gateway and Virtual Routing and Forwarding](#virtual-private-gateway-and-virtual-routing-and-forwarding)
- [References](#references)


# Overview
- [Source](https://d1.awsstatic.com/whitepapers/Networking/integrating-aws-with-multiprotocol-label-switching.pdf)

This summary is based off of the December 2016 revision of the **Integrating AWS with Multiprotocol Label Switching** whitepaper. This whitepaper provides best practices for integrating a customer's existing [Multiprotocol Label Switching (MPLS)](https://www.cloudflare.com/en-gb/learning/network-layer/what-is-mpls/) provider with AWS in single or multi-regional configurations.

# Introduction
Many service providers offer a managed MPLS solution that is either Layer 3 or Layer 2 based [as according to OSI model](https://www.cloudflare.com/en-gb/learning/ddos/glossary/open-systems-interconnection-model-osi/), and the solution provides a logical extension of a customer's network. In this whitepaper, MPLS refers to the service provider's managed MPLS/[WAN](https://www.cloudflare.com/en-gb/learning/network-layer/what-is-a-wan/) solution. While AWS does not natively integrate with MPLS as a protocol, AWS provides mechanims by which to connect an existing MPLS/WAN solution via [AWS Direct Connect](https://aws.amazon.com/directconnect/) and VPN.

# Overview of AWS Networking Services and Core Technologies
This section briefly describes the key AWS services and technologies discussed in this whitepaper.

## Amazon Virtual Private Cloud
An [Amazon Virtual Private Cloud (VPC)](https://aws.amazon.com/vpc/) is a logically isolated virtual network in which a customer can launch AWS resources (such as virtual machines) and define its IP addressing scheme. Routing tables, network gateways, subnet ranges, and network firewalls are configured along with the VPC. A simplified VPC architecture is shown below.

![VPC](../Diagrams/VPC.png)

## AWS Direct Connect and VPN
VPCs can be connected to from on-premises networks via AWS Direct Connect and/or VPN connections using any IPsec/IKE-compliant platform (routers and firewalls). These connectivity options are discussed in great detail in the [*VPC Connectivity Options*](./vpc-connectivity-options.md) summary.

## Internet Gateway
The [Internet Gateway (IGW)](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) is a VPC component that allows communication between instances in the VPC and the Internet. It must be explicitly routed to by a routing table to be used.

## Customer Gateway
The [Customer Gateway (CGW)](http://docs.aws.amazon.com/AmazonVPC/latest/NetworkAdminGuide/Introduction.html) is a VPC component that represents the customer's side of the connection between their network and the VPC. In an MPLS scenario, the CGW can be either: 
- a customer edge (CE) device located at a Direct Connect location, or 
- a provider edge (PE) device in an existing MPLS VPN network

## Virtual Private Gateway and Virtual Routing and Forwarding
The [Virtual Private Gateway (VGW)](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html#VPN) is the AWS Managed VPN endpoint of the connection between a customer's network and a VPC. The customer enables an IPsec VPN connection from their CGW to the VGW. 

It's also possible to connect from an on-premises network to VPCs using a [virtual routing and forwarding (VRF)](https://en.wikipedia.org/wiki/Virtual_routing_and_forwarding) approach. AWS recommends implementing a VRF if a customer has concerns about IP overlap with multiple VPCs.


# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/Networking/integrating-aws-with-multiprotocol-label-switching.pdf)
- [What is Amazon VPC? (AWS User Guide)](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [What is MPLS? (Cloudflare)](https://www.cloudflare.com/en-gb/learning/network-layer/what-is-mpls/)
- [What is the OSI Model? (Cloudflare)](https://www.cloudflare.com/en-gb/learning/ddos/glossary/open-systems-interconnection-model-osi/)
- [What is a WAN? (Cloudflare)](https://www.cloudflare.com/en-gb/learning/network-layer/what-is-a-wan/)