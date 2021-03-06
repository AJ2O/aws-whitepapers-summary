# **AWS Well-Architected Framework**

# Sections
- [**AWS Well-Architected Framework**](#aws-well-architected-framework)
- [Sections](#sections)
- [Overview](#overview)
- [Definitions](#definitions)
  - [Terminology](#terminology)
  - [The Five Pillars](#the-five-pillars)
- [General Design Principles](#general-design-principles)
- [The Five Pillars of the Framework](#the-five-pillars-of-the-framework)
- [References](#references)

# Overview
- [Source](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)

This summary is based off of the July 2020 revision of the **AWS Well-Architected Framework** whitepaper.

The AWS Well-Architected Framework (W-AF) helps customers learn the best architectural practices for designing and operating systems on the cloud. For a customer, it provides a way to measure their current cloud architecture decisions against best practices to identify areas for improvement, which will likely lead to increased business success.

The W-AF poses fundamental questions to the customer to help them evaluate if their system has the expectant qualities of modern cloud-based systems, and the remediation steps to achieve those qualities.

# Definitions

## Terminology
- **Component**
  - The code, configuration, and AWS resources that together deliver against a requirement. It is often the smallest unit of technical ownership.
  - Examples: a MySQL database, a load balancer, a web server
- **Workload** 
  - A set of components that together deliver business value. This is usually the level of detail that business and technology leaders communicate about.
  - Example: An application consisting of a load balancer, web servers and a MySQL database
- **Technology portfolio** 
  - The collection of workloads that are required for the business to operate.
- **Milestones** 
  - Key changes in the architecture as it evolves throughout the product lifecycle: design, implementation, testing, go live, and production.
- **Architecture** 
  - The manner in which multiple components work together to form a workload. How components communicate with each other is usually the focus of architecture diagrams.
  - Example: Simple architecture diagram of the workload example above

![SimpleArchitecture](./Diagrams/SimpleArchitecture.png)

## The Five Pillars
The WA-F is based on five pillars, **operational excellence**, **security**, **reliability**, **performance efficiency**, and **cost optimization**. They're each described in the table below, and will be explained further in their own sections.

<html>
<table>
  <tr>
    <th width="200">Pillar</th>
    <th width="600">Description</th>
  </tr>
  <tr>
    <th>Operational Excellence</th>
    <td>The ability to support development and run workloads effectively, gain insight into their operations, and to continuously improve supporting processes and procedures to deliver business value.</td>
  </tr>
  <tr>
    <th>Security</th>
    <td>The ability to protect data, systems, and assets to take advantage of cloud technologies to improve the organization's security.</td>
  </tr>
  <tr>
    <th>Reliability</th>
    <td>The ability of a workload to perform its intended function correctly and consistently when it's expected to. This includes the ability to operate and test the workload through its total lifecycle.</td>
  </tr>
  <tr>
    <th>Performance Efficiency</th>
    <td>The ability to use computing resources efficiently to meet system requirements, and to maintain that efficiency as demand changes and technologies evolve.</td>
  </tr>
  <tr>
    <th>Cost Optimization</th>
    <td>The ability to run systems to deliver business value at the lowest price point.</td>
  </tr>
</table>
</html>

When architecting workloads, sometime tradeoffs have to be made between pillars based on the business context. Generally however, operational excellence and security are not traded off.

# General Design Principles
The W-AF identifies some general principles to facilitate good design in the cloud:
- **Stop guessing capacity needs**
- **Test systems at production scale**
  - The cloud capability to test on demand means a user can set up a production-scale test environment, complete the testing, then decomission the environment once finished
  - Costs are only accrued while the environment is running, allowing for simulating the live environment for a fraction of the cost on-premises
- **Automate to make architectural experimentation easier**
- **Allow for evolutionary architectures**
  - The cloud capability to test on demand lowers the risk of impact from design changes, allowing more room for innovation
- **Drive architectures using data**
- **Improve through game days**
  - Regularly schedule simulation events in production to understand where improvements can be made, and to gain experience dealing with events

# The Five Pillars of the Framework

# References
- [AWS Well-Architected Framework (Documentation)](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)