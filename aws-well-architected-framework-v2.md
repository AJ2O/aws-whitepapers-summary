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
  - [Operational Excellence](#operational-excellence)
    - [Design Principles](#design-principles)
    - [Definition](#definition)
    - [Best Practices](#best-practices)
      - [Organization](#organization)
      - [Prepare](#prepare)
      - [Operate](#operate)
      - [Evolve](#evolve)
  - [Security](#security)
  - [Reliability](#reliability)
  - [Performance Efficiency](#performance-efficiency)
  - [Cost Optimization](#cost-optimization)
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
## Operational Excellence

### Design Principles
These design principles will be prevalent for operational excellence best practices:
- **Perform operations as code**
- **Make frequent, small, reversible changes**
- **Refine operations procedures frequently**
- **Anticipate failure**
- **Learn from all operational failures**

### Definition
The ability to support development and run workloads effectively, gain insight into their operations, and to continuously improve supporting processes and procedures to deliver business value.

There are four best practice areas for operational excellence in the cloud:
- [Organization](#organization)
- [Prepare](#prepare)
- [Operate](#operate)
- [Evolve](#evolve)

### Best Practices

#### Organization
Teams need to have a shared understanding of the entire workload, their role in it, and shared business goals to set well-defined priorities that will enable business success. 

**OPS 1: How do you determine what your priorities are?**
- Evaluate internal and external customer needs involving key stakeholders, including business, development, and operations teams. This will ensure that you have a thorough understanding of the operations support that is required to achieve business outcomes.
- Evaluate the threat landscape and keep a registry of risks. Include their impacts to determine where to focus efforts.
- Manage benefits and risks to make informed decisions about where to focus efforts. Some risks can be mitigated or even acceptable for a time, while others would need immediate remediation if they were to occur.

**OPS 2: How do you structure your organization to support your business outcomes?**
- Processes, procedures, and resources have identified owners, and a defined business value or reasoning for existence.
- Understanding the responsibilities of your role and how you contribute to business outcomes informs the prioritization of your tasks and why your role is important.
- Responsibilities between teams are predefined, describing how they work with and support each other. Understanding each teams' work on business outcomes informs the prioritization of their tasks, and enables them to respond appropriately.

**OPS 3: How does your organizational culture support your business outcomes?**
- Senior leadership clearly sets expectations for the organization and evaluates success.
- Communications between team members and teams are timely, clear, and actionable.
- Team members are enabled and encouraged to maintain and grow their skill sets. Growth of skills in new technologies is frequently a source of team member satisfaction and supports innovation.
- Resource teams appropriately, with team member capacity and tools for them to support their workload needs.

#### Prepare
To prepare for operational excellence, team members need to understand their workloads and their expected behaviours. Then the workloads can be designed to provide insight into their status, and procedures can be built to support them.

**OPS 4: How do you design your workload so that you can understand its state?**
- Instrument the application code to emit information about its internal state, status, and achievement of business outcomes.
  - This is known as *application telemetry*.
- Configure *workload telemetry*, such as HTTP status codes, auto-scaling events, API call volume, etc.
- Implement *user activity telemetry* to emit information about user activities, such as click streams, started or completed transactions, etc.
- Implement *transaction telemetry*, to track transactions across the workload

**OPS 5: How do you reduce defects, ease remediation, and improve flow into production?**
- Test and validate changes to help limit and detect errors. Automate testing where possible to reduce manual error.
- Make frequent, small, reversible code changes to reduce the scope and impact of change and potential errors
- Use automated management systems for: configuration, build and deployment, and patching
  - Reduces the amount of manual errors and the effort required for each.

**OPS 6: How do you mitigate deployment risks?**
- Prepare for unsuccessful changes by having a rollback plan to revert to a known good state.
- Deploy using parallel environments. Once the new deployment is confirmed as successful, the prior environment can be removed.
- Fully automate integration and deployment to reduce manual error and effort.

**OPS 7: How do you know that you are ready to support a workload?**
- Ensure that you have the appropriate number of trained personnel to provide support for operational needs.
- Ensure consistent review of operational readiness, including at minimum, the operational readiness of the team and workload, and the security requirements.
- Use documented procedures (called *runbooks*) to achieve specific outcomes for well-understood events. Implement runbooks as code and trigger them in response to events where appropriate to ensure speed and consistency, while reducing  manual error.
- Document the investigation process of issues not well known, in *playbooks*. Playbooks are predefined steps to perform to identify factors contributing to a failure scenario.

#### Operate
Successful operation of a workload is measured by the achievement of business and customer outcomes. Define expected outcomes, determine how success will be measured, and identify the metrics that will be used in those calculations to determine workload and operations success.

**OPS 8: How do you understand the health of your workload?**
- Identify *key performance indicators (KPIs)* based on desired business outcomes and customer outcomes, and evaluate the KPIs to determine workload success.
- Define, collect, and analyze workload metrics:
  - Metrics to measure KPI achievement (for example, abandoned shopping carts, orders placed, cost, price).
  - Metrics to measure workload health (for example, response time, error rate, requests made and completed).
- Alert when workload outcomes are at risk, or anomalies are detected.
- Validate the achievement of outcomes and the effectiveness of KPIs and metrics.

**OPS 9: How do you understand the health of your operations?**
- Identify KPIs based on desired business (features delivered etc.) and customer (support cases etc.) outcomes, and evaluate the KPIs to determine operations success.
- Define, collect, and analyze operations metrics:
  - Metrics to measure KPI achievement (for example, successful deployments, failed deployments)
  - Metrics to measure health of operations activities (for example, mean time to detect an incident (MTTD), mean time to recovery (MTTR) from an incident)
- Establish operations metrics baselines, and learn expected patterns of activity
  - Alert when anomalies are detected, or operations outcomes are at risk
- Validate the achievement of outcomes and the effectiveness of KPIs and metrics.

**OPS 10: How do you manage workload and operations events?**
- Prepare and validate procedures for responding to events to minimize their disruption to your workload.
- Prioritize operational events based on business impact.
- Define escalation paths in your runbooks and playbooks, and identify owners for each action to ensure prompt responses to events.
- Where possible, automate responses to events.
- Communicate status through dashboards tailored to their target audiences.

#### Evolve
You must learn, share, and continuously improve to sustain operational excellence.

**OPS 11: How do you evolve operations?**
- Dedicate time and resources for continuous incremental improvement to evolve the effectiveness and efficiency of your operations.
- Have a process for continuous improvement by regularly evaluating and prioritizing opportunities to focus efforts where they can provide the greatest benefits.
- Review customer-impacting events, and identify the contributing factors and preventative actions. This information can be used to develop mitigations to limit or prevent future occurrence.
- Regularly perform retrospective analysis of operations metrics with cross-team participants to identify opportunities for improvement, potential courses of action, and to share lessons learned.
- Document and share lessons learned from operations activities so that they can be used internally and across teams.

## Security

## Reliability

## Performance Efficiency

## Cost Optimization

# References
- [AWS Well-Architected Framework (Documentation)](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)