# **An Overview of the AWS Cloud Adoption Framework**

# Sections
- [**An Overview of the AWS Cloud Adoption Framework**](#an-overview-of-the-aws-cloud-adoption-framework)
- [Sections](#sections)
- [Overview](#overview)
- [Introduction](#introduction)
- [Mapping the Journey to the Cloud](#mapping-the-journey-to-the-cloud)
- [AWS CAF Perspectives: Additional Detail](#aws-caf-perspectives-additional-detail)
  - [Business Perspective: Value Realization](#business-perspective-value-realization)
  - [People Perspective: Roles and Readiness](#people-perspective-roles-and-readiness)
  - [Governance Perspective: Prioritization and Control](#governance-perspective-prioritization-and-control)
  - [Platform Perspective: Applications and Infrastructure](#platform-perspective-applications-and-infrastructure)
  - [Security Perspective: Risk and Compliance](#security-perspective-risk-and-compliance)
  - [Operations Perspective: Manage and Scale](#operations-perspective-manage-and-scale)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)

This summary is based off of the February 2017 revision of the **An Overview of the AWS Cloud Adoption Framework** whitepaper. It explains how the AWS Cloud Adoption Framework (CAF) can assist organizations with the nuances, considerations, and changes that must be recognized in order to maximize benefit from cloud adoption.

# Introduction
Cloud adoption doesn't only entail technological changes, but also changes throughout all organizational units in the company. The CAF provides guidance that supports each unit so that each business area understands how to update skills, adapt existing processes, and use new processes that take full advantage of cloud computing services. CAF organizes guidance into **six Perspectives**, which are shown below:

- [**Business**](#business-perspective-value-realization)
- [**People**](#people-perspective-roles-and-readiness)
- [**Governance**](#governance-perspective-prioritization-and-control)<br><br>
- [**Platform**](#platform-perspective-applications-and-infrastructure)
- [**Security**](#security-perspective-risk-and-compliance)
- [**Operations**](#operations-perspective-manage-and-scale)

In general, the Business, People, and Governance Perspectives focus on **business capabilities**, and the Platform, Security, and Operations Perspectives focus on **technical capabilities**. A brief description of each Perspective is below, with detailed descriptions in later sections.

<html>
<table align="center">
    <tr>
        <th align="center" width="40">Perspective</th>
        <th align="center" width="200">Common Roles</th>
        <th align="center" width="400">Helps stakeholders understand how to update the staff skills and organizational processes necessary to...</th>
    </tr>
    <tr>
        <th align="center">Business</th>
        <td>Business Managers, Budget Owners, Strategy Stakeholders</td>
        <td>... optimize business value in the cloud.</td>
    </tr>
    <tr>
        <th align="center">People</th>
        <td>HR, Staffing, People Managers</td>
        <td>... optimize and maintain their workforce.</td>
    </tr>
    <tr>
        <th align="center">Governance</th>
        <td>CIO, Project Managers, Enterprise Architects, Business Analysts, Portfolio Managers</td>
        <td>... ensure business governance in the cloud, and manage and measure cloud investments to
        evaluate their business outcomes.</td>
    </tr>
    <tr>
        <th align="center">Platform</th>
        <td>CTO, IT Managers, Solution Architects</td>
        <td>... deliver and optimize cloud solutions and services.</td>
    </tr>
    <tr>
        <th align="center">Security</th>
        <td>CISO, IT Security Managers, IT Security Analysts</td>
        <td>... ensure that deployed cloud architecture aligns to the organization's requirements for
        security control, resilience, and compliance.</td>
    </tr>
    <tr>
        <th align="center">Operations</th>
        <td>IT Operations Managers, IT Support Managers</td>
        <td>... ensure system health and reliability during move of operations to the cloud, and then
        to operate using agile, ongoing, cloud computing best practices.</td>
    </tr>
</table>
<html>

# Mapping the Journey to the Cloud
Engaging stakeholders with their relevant CAF Perspective helps inform an organization's journey to the cloud. Identifying the capability gaps in terms of the CAF, defining necessary work streams, and identifying interpendencies between streams enables the organization to optimize collaboration on AWS. Work streams are iterative and change over time, and sometimes it is best for work streams to be integrated with one another (such as [DevOps](../DevOps/introduction-to-devops.md) for example). 

As work streams are implemented, the organization can leverage the different CAF Perspectives to understand how to communicate work stream interdependencies between stakeholders. The CAF structure can help ensure that the strategies and plans across the organization are complete and aligned to business goals and outcomes.

# AWS CAF Perspectives: Additional Detail

## Business Perspective: Value Realization
The **Business Perspective** is focused on ensuring that IT is aligned with business needs and that IT investments can be traced to demonstrable business results.

### CAF Capabilities

- [IT Finance](#it-finance)
- [IT Strategy](#it-strategy)
- [Benefits Realization](#benefits-realization)
- [Business Risk Management](#business-risk-management)

#### IT Finance
Addresses the organization's capability to **plan, allocate, and manage the budget for IT expenses** given changes introduced with the cloud services consumption model.

#### IT Strategy
Focuses on the organization's capability to **leverage IT as a business enabler.** In many organizations, IT has devolved into simply ensuring collaboration and line-of-business applications stay healthy and operational. IT teams may need new skills to gather business requirements and new processes to solve business challenges.

#### Benefits Realization
Encompasses the organization's capability to **measure the benefits received from its IT investments.** For many organizations, this represents Total Cost of Ownership (TCO) or Return on Investment (ROI) coupled with budget management.

#### Business Risk Management
Focuses on the organization's capability to **understand the business impact of preventable, strategic, and external risks** to the organization. For many, these risks stem from the impact of financial and technology constraints on agility. Many of these constraints are alleviated or eliminated with a move to the cloud.

## People Perspective: Roles and Readiness
The **People Perspective** covers organizational staff capability and change management functions required for efficient cloud adoption.

### CAF Capabilities

- [Resource Management](#resource-management)
- [Incentive Management](#incentive-management)
- [Career Management](#career-management)
- [Training Management](#training-management)
- [Organizational Change Management](#organizational-change-management)

#### Resource Management
Addresses the organization's capability to **project personnel needs** and to **attract and hire the talent necessary to support the organization's goals.** Cloud adoption requires that staffing teams need to acquire new skills necessary to understand cloud technologies, and they may need to update processes for forecasting future staffing requirements.

#### Incentive Management
Addresses the organization's capability to **ensure that its workers receive competitive compensation and benefits** for the value they bring to it. With the shift to cloud, some IT roles move from being commoditized to being highly specialized with high market demand.

#### Career Management
Focuses on the organization's capability to **ensure the personal fulfillment** of its employees, their career opportunities, and their financial security.

#### Training Management
Addresses the organization's capability to **ensure employees have the knowledge and skills to perform their roles and comply** with organizational policies and requirements.

#### Organizational Change Management
Focuses on the organization's capability to **manage the effects and impacts of business, structural, and cultural change** introduced with cloud adoption. Clear communication is critical to ease change and reduce uncertainty that may be present for staff when introducing new ways of working.

## Governance Perspective: Prioritization and Control
The **Governance Perspective** focuses on the skills and processes needed to align IT strategy and goals with the organization's business strategy and goals, to ensure maximized business value of its IT investment, and minimized business risks.

### CAF Capabilities

- [Portfolio Management](#portfolio-management)
- [Program and Project Management](#program-and-project-management)
- [Business Performance Measurement](#business-performance-measurement)
- [License Management](#license-management)

#### Portfolio Management
Focuses on the organization's capability to **manage and prioritize IT investments, programs, and projects in alignment with its business goals.** It is an important mechanism to determine cloud-eligibility for workloads and serves as a focal point for lifecycle management of both applications and services.

#### Program and Project Management
Addresses the organization's capability to **manage one or several related projects** to improve organizational performance and complete the projects **on time and on budget.**

#### Business Performance Measurement
Addresses the organization's capability to **measure and optimize processes in support of its goals.** Cloud services offer the potential for rapid experimentation with new means of process automation and optimization. New processes to define cloud-centric KPIs is necessary to leverage this potential.

#### License Management
Defines the organization's capability to **procure, distribute, and manage the licenses needed for IT systems, services, and software.** The cloud consumption model requires that teams develop new skills for procurement and license management and new processes for evaluating license needs.

## Platform Perspective: Applications and Infrastructure
The **Platform Perspective** focuses on describing the structure and design of all types of cloud architectures.

### CAF Capabilities

- [Compute Provisioning](#compute-provisioning)
- [Network Provisioning](#network-provisioning)
- [Storage Provisioning](#storage-provisioning)
- [Database Provisioning](#database-provisioning)
- [Systems and Solution Architecture](#systems-and-solution-architecture)
- [Application Development](#application-development)

#### Compute Provisioning
Encompasses the organization's capability to **provide the processing and memory** in support of enterprise applications. The processes and skills to provision cloud services are different than those needed to provision physical hardware and manage data center facilities.

#### Network Provisioning
Addresses the organization's capability to **provide computing networkins** to support enterprise applications.

#### Storage Provisioning
Focuses on the organization's capability to **provide storage** in support of enterprise applications.

#### Database Provisioning
Addresses the organization's capability to **provide databases and database management systems** in support of enterprise applications.

#### Systems and Solution Architecture
Encompasses the organization's capability to **define and describe the design of the system and to create architecture standards** for an organization.

#### Application Development
Defines the organization's capability to **customize or develop applications** to support its business goals. New skills and processes for [Continuous Integration and Continuous Deployment (CI/CD)](../DevOps/practicing-cicd.md) are a critical part of designing applications that take advantage of cloud services and the agility promised by cloud computing.

## Security Perspective: Risk and Compliance
The **Security Perspective** helps an organization to structure the selection and implementation of security controls that meet its needs.

### CAF Capabilities

- [Identity and Access Management](#identity-and-access-management)
- [Detective Control](#detective-control)
- [Infrastructure Security](#infrastructure-security)
- [Data Protection](#data-protection)
- [Incident Response](#incident-response)

#### Identity and Access Management
This capability enables an organization to **create multiple access control mechanisms and manage the permissions for each** within the organization's AWS account.

#### Detective Control
AWS provides the capability for **native logging** as well as services that can be leveraged to **provide greater visibility to near real time** for occurrences in the AWS environment.

#### Infrastructure Security
This capability provides the opportunity for the organization to **shape its AWS security controls in an agile fashion;** automating the ability to build, deploy, and operate its security infrastructure.

#### Data Protection
Addresses the organization's capability for **maintaining visibility and control over data**, and **how it is accessed and used** in the organization.

#### Incident Response
Focuses on the organization's capability to **respond, manage, reduce harm, and restore operations** after a security incident.

## Operations Perspective: Manage and Scale
The **Operations Perspective** describes the focus areas used to enable, run, use, operate, and recover IT workloads to the level that is agreed upon with business stakeholders.

### CAF Capabilities

- [Service Monitoring](#service-monitoring)
- [Application Performance Monitoring](#application-performance-monitoring)
- [Resource Inventory Management](#resource-inventory-management)
- [Release Management / Change Management](#release-management--change-management)
- [Reporting and Analytics](#reporting-and-analytics)
- [Business Continuity / Disaster Recovery (BC/DR)](#business-continuity--disaster-recovery-bcdr)
- [IT Service Catalog](#it-service-catalog)

#### Service Monitoring
Addresses the organization's capability to **detect and respond to issues** with the health IT services and enterprise applications. With cloud adoption, detection and response can be highly automated, resulting in greater service uptime.

#### Application Performance Monitoring
Addresses the organization's capability to **ensure application performance meets its defined requirements.**

#### Resource Inventory Management
Addresses the organization's capability to **organize its assets** in a way that provides the **best, most cost-efficient service.** Cloud adoption removes the need to manage hardware assets and the hardware lifecyle, and instead leverages on-demand techniques that optimize license usage.

#### Release Management / Change Management
Encompasses the organization's capability to **manage, plan, and schedule changes to the IT environment.** Cloud adoption provides the opportunity to leverage CI/CD techniques to rapidly manage releases and roll-backs.

#### Reporting and Analytics
Addresses the organization's capability to **ensure compliance with its reporting policies** and to **ensure ongoing analysis and reporting of performance against key KPIs** such as service-level agreements (SLAs) and operational-level agreements (OLAs).

#### Business Continuity / Disaster Recovery (BC/DR)
Addresses the organization's capability to **operate in the event of a significant failure of IT services**, and to **recover from those failures within the time parameters** defined by the organization.

#### IT Service Catalog
The organization's capability to **select, maintain, advertise, and deliver an SLA or set of IT services.** It is closely coupled with [Portfolio Management](#portfolio-management) in order to ensure that technical services are aligned to business goals and needs.

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
