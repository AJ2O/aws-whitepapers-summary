# **Amazon Virtual Private Cloud Connectivity Options**

# Sections
- [**Amazon Virtual Private Cloud Connectivity Options**](#amazon-virtual-private-cloud-connectivity-options)
- [Sections](#sections)
- [Overview](#overview)
- [Network-to-Amazon VPC Connectivity Options](#network-to-amazon-vpc-connectivity-options)
  - [Option Comparison](#option-comparison)
  - [AWS Managed VPN](#aws-managed-vpn)
    - [How It Works](#how-it-works)
  - [AWS Direct Connect](#aws-direct-connect)
  - [AWS Direct Connect + VPN](#aws-direct-connect--vpn)
  - [AWS VPN CloudHub](#aws-vpn-cloudhub)
  - [Software VPN](#software-vpn)
  - [Transit VPC](#transit-vpc)
- [Amazon VPC-to-Amazon VPC Connectivity Options](#amazon-vpc-to-amazon-vpc-connectivity-options)
  - [Option Comparison](#option-comparison-1)
- [Internal User-to-Amazon VPC Connectivity Options](#internal-user-to-amazon-vpc-connectivity-options)
- [Conclusion](#conclusion)
- [References](#references)


# Overview
- [Source](https://d1.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf)

This summary is based off of the January 2018 revision of the **Amazon Virtual Private Cloud Connectivity Options** whitepaper. This whitepaper describes network connectivity options for [Amazon Virtual Private Cloud (VPC)](https://aws.amazon.com/vpc/) available on AWS. These options include integrating remote customer networks with VPCs and joining multiple VPCs into a connected virtual network.

# Network-to-Amazon VPC Connectivity Options
These options are useful for integrating AWS resources with existing on-premises services, applications and servers. It also allows internal users to interact and connect with the AWS-hosted resources just like any other on-premises resource.

Below is a comparison chart summarizing each option, including their advantages and disadvantages. Each option is explained in greater detail in subsequent sections.

## Option Comparison
<html>
    <table>
        <tr>
            <th align="center" width="160">Option</th>
            <th align="center" width="160">Use Case</th>
            <th align="center" width="300">Advantages</th>
            <th align="center" width="300">Disadvantages</th>
        </tr>
    </table>
</html>

## AWS Managed VPN
This option is used to establish an IPsec VPN connection between on-premises networks and a VPC over the Internet. The diagram below shows what this architecture looks like.

![AWSVPN](../Diagrams/AWSVPN.png)

### How It Works
**1. Virtual Private Gateway**
- The virtual private gateway is the VPN concentrator on the AWS side of the VPN connection and is created by the customer
- It is attached to the VPC that is to be connected to by on-premises networks
  
**2. Customer Gateway**
- The customer gateway is an AWS resource representing the VPN device on the on-premises side of the VPN connection
- When being created, the customer provides information about their device to AWS

There is built-in multi-data center redundancy and failover for the virtual private gateway to ensure availability of the VPN connection. It is recommended that the customer creates multiple customer gateway connections to ensure availability on their side of the VPN connection.

Both dynamic (BGP peering), and static routing options are provided to give the customer flexibility on their routing configuration.

- To read more about configuring a VPN connection to VPCs from on-premises networks, read [*How AWS Site-to-Site VPN works*](https://docs.aws.amazon.com/vpn/latest/s2svpn/how_it_works.html) in the AWS VPN user guide
- To read about the customer gateway device minimum requirements to work with VPCs and some examples, read the [*Your customer gateway device*](https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#example-configuration-files) section

## AWS Direct Connect



## AWS Direct Connect + VPN

## AWS VPN CloudHub

## Software VPN

## Transit VPC

# Amazon VPC-to-Amazon VPC Connectivity Options

Below is a comparison table summarizing each option, including their advantages and disadvantages. Each option is explained in greater detail in subsequent sections.

## Option Comparison
<html>
    <table>
        <tr>
            <th align="center" width="160">Option</th>
            <th align="center" width="160">Use Case</th>
            <th align="center" width="300">Advantages</th>
            <th align="center" width="300">Disadvantages</th>
        </tr>
    </table>
</html>

# Internal User-to-Amazon VPC Connectivity Options

# Conclusion

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/aws-amazon-vpc-connectivity-options.pdf)