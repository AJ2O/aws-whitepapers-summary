# **Reliability Pillar - AWS Well-Architected Framework**

# Sections
- [**Reliability Pillar - AWS Well-Architected Framework**](#reliability-pillar---aws-well-architected-framework)
- [Sections](#sections)
- [Overview](#overview)
  - [Prerequisites](#prerequisites)
- [Definitions](#definitions)
  - [Resiliency](#resiliency)
- [Foundations](#foundations)
- [Workload Architecture](#workload-architecture)
- [Change Management](#change-management)
- [Failure Management](#failure-management)
- [Conclusion](#conclusion)

# Overview
- [Source](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html)

This summary is based off of the July 2020 revision of the **Reliability Pillar - AWS Well-Architected Framework** whitepaper.

The AWS Well-Architected Framework (W-AF) helps customers learn the best architectural practices for designing and operating systems on the cloud. For a customer, it provides a way to measure their current cloud architecture decisions against best practices to identify areas for improvement, which will likely lead to increased business success. The W-AF consists of five pillars, and they are summarized in [its own whitepaper](./well-architected-framework.md).

This whitepaper in particular focuses on the **reliability** pillar of the W-AF, and how to apply it to cloud solutions. It encompasses the ability of a workload to perform its intended function correctly and consistently when it's expected to. This includes the ability to operate and test the workload through its total lifecycle. This paper provides in-depth, best practice guidance for implementing reliable workloads on AWS.

## Prerequisites
- There is a shortened version of this document in the larger context of the W-AF, which contains the main points of reliability. Read [the reliability section](./well-architected-framework.md#the-five-pillars-reliability) first before reading this document. Many of the broader points already outlined there will not be repeated here.

# Definitions

## Resiliency
Reliability of a workload in the cloud depends on several factors, the primary of which is **Resiliency:**
- **Resiliency** is the ability of a workload to:
  - Recover from infrastructure or service disruptions
  - Dynamically acquire computing resources to meet demand
  - Mitigate disruptions

The other four pillars of the AWS W-AF can also have impacts on reliability:
- **Operational Excellence:** Using runbooks and playbooks to investigate and quickly respond to failure scenarios can confirm that applications are ready for production.
- **Security:** Workloads must be protected from malicious actors who could impact availability.
- **Performance Efficiency:** designing for maximum request rates and minimizing latencies can stabilize load induced on applications.
- **Cost Optimization:** tradeoffs could be made to spend more on EC2 instances to achieve more stability.

While the other four aspects are important to reliability, they are already covered in the [AWS WA-F](./well-architected-framework.md), so resiliency will be the primary focus of this document.


# Foundations

# Workload Architecture

# Change Management

# Failure Management

# Conclusion