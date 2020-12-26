# AWS Well-Architected Framework

## Overview
- [Source Whitepaper](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)

## Definitions

### Common Terminology
**Component**
- Code, configuration, and AWS resources that deliver against a requirement
- The smallest "unit", decoupled from other components
  
**Workload**
- The set of components that together deliver business value
- Is the level of detail communicated about by business leaders

**Architecture**
- Describes how components work together in a workload, sometimes using the help of a diagram

### The Five Pillars of the Framework - PROCS / CROPS
**Performance Effciency**
- The ability to: 
  - Use computing resources to meet requirements
  - Maintain efficiency as demand and technologies change

**Reliability**
- The ability of a workload to perform its intended function correctly and consistently over its lifecycle

**Operational Excellence**
- The ability to:
  - Support development and run workloads effectively
  - Gain insight into workload operations
  - Continuously improve supporting processes to deliver business value

**Cost Optimization**
- The ability to deliver business value at the lowest price

**Security**
- The ability to:
  - Protect data, systems, and assets
  - Leverage cloud technologies to improve security

## 5P - Performance Efficiency
### Design Principles
**Delegate Advanced Technology Tasks to Cloud Vendor**
- Ex. Storage (S3), Media Transcoding (Elastic Transcoder), Machine Learning (SageMaker, Lex, Rekognition, etc.) and more, are all managed services
- Dev teams don't need to learn about hosting and running these technologies on servers
  - Can focus on product development

**Use Serverless Architectures**
- No need to manage underlying servers, and take advantage of scaling and high availability
  - Ex:
    - Hosting a static website on S3 instead of a server
    - Managed NoSQL database service in DynamoDB
    - CI/CD tools such as CodePipeline

**Experiment More Often**
- Cloud resources are virtual and automatable, so comparative testing can be quickly achieved
  - Ex. Quick to compare instance configurations or storage solutions

**Mechanical Sympathy**
- Consider how cloud services are used, and select the most appropriate service for your workload

### Best Practices

**Selection**
- Well-Architected systems use multiple solutions and features to improve performance
  - The best solution for a workload varies depending on its components
  - Each component may use different resources and configurations to optimize performance
- Compute solutions can used for different components, but the wrong selection leads to performance degradation
- Storage solutions include object (S3), block (EBS), and file (EFS) storage
  - The optimal solution should be selected based on the application's data access patterns
- Database solutions should be selected based on features such as latency, query capability, consistency, etc.

**Review**
- Take advantage of continually evolving cloud innovation
- New services and features can positively increase performance efficiency

**Monitoring**
- System performance can degrade over time
- Identify degradation and remediate internal or external factors
  - Ex. Operating system, application load

**Tradeoffs**
- Performance is often optimized by trading consistency, durability, or space for latency

## 5P - Reliability
### Design Principles

**Automatically Recover From Failure**
- Monitor workloads for Key Performance Indicators (KPIs), and trigger automation once a threshold is breached
  - KPIs should measure business value, not simply technical aspects of the infrastructure

**Test Recovery Procedures**
- In the cloud, you can simulate different failure scenarios, and test recovery procedures
  - Exposes testable failure pathways that can be remediated before a real scenario occurs

**Scale Horizontally to Increase Workload Availability**
- Split one large resource into multiple, smaller resources, and distribute workload tasks among them
  - Reduces the impact of a single failure on the overall workload

**Stop Guessing Capacity**
- Many cloud services implement auto-scaling and self-healing to maintain the optimal amount of resources without under- or over-provisioning

**Manage Changes with Automation**
- Changes to infrastructure should be made using automation
  - Is consistent
  - Can be tracked and reviewed

### Best Practices

**Foundations**
- Foundational requirements that influence reliably must be addressed before architecting any system
  - Ex. Sufficient bandwith to your data center
- AWS handles most requirements, or they can be addressed as needed
  - Ex. Increasing service quotas

**Workload Architecture**
- Build highly scalable and reliable workloads using a Service-Oriented Architecture (SOA) or microservices architecture
- These distributed components operate in a way to not negatively impact other components in the workload
  - Workload withstands component stresses or failures, and can quickly recover

**Change Management**
- Changes to the workload must be accomodated for to maintain reliability
- Use logs and metrics to monitor resources
- Enable elasticity

**Failure Management**
- Take advantage of automation to react to monitored data
- Where possible, implement:
  - Regular backups, fault isolation boundaries, disaster recovery (DR) plans

## 5P - Operational Excellence
### Design Principles
**Perform Environment Operations as Code**
- Apply the same discipline for application to your cloud environment
  - The entire workload and environment can be defined and updated as code
  - Ex. CloudFormation
- Reduces human error, and enables consistency

**Make Frequent, Small, Reversible Changes**
- Design workload components to be updated regularly
- Small changes can be easily reversed if they cause failure

**Refine Operations Procedures Frequently**
- As the workload evolves, evolve operation procedures accordingly

**Anticipate Failure**
- Regularly test failure scenarios and validate associated response procedures, so teams are familiar with their execution

**Learn From All Operational Failures**
- Share what is learned across teams and the organization to drive improvement

### Best Practices

**Organization**
- Your organization's teams need a shared understanding of the entire workload, their roles in it, business goals, and priorities to enable business success
- Teams need to understand the role of other teams in their success, and have shared goals
- Helps focus efforts and maximizes benefits from all teams

**Prepare**
- Design workloads to provide the information necessary to understand its internal state
  - Ex. metrics, logs, events, traces
- Adopt approaches that:
  - Improve the flow of changes into production
  - Enable refactoring
  - Fast feedback on quality, bug fixing, and rapid recovery
- Evaluate and understand the operational risks of the workload before deploying

**Operate**
- Define expected outcomes and how success is measured, and identify the metrics used in those calculations
  - Evaluate workload metrics to determine the health of the workload
  - Evaluate operations metrics to determine the health of operations events

**Evolve**
- Dedicate time and resources for continuous incremental improvement of your operations

## 5P - Cost Optimization
### Design Principles
- **Implement Cloud Financial Management**
  - Your organization needs to dedicate time and resources to learn how to become cost-efficient in the cloud and increase business value
    - Learned through knowledge building, programs, resources, etc.

- **Adopt a Consumption Model**
  - Pay only for required resources, and scale usage depending on business requirements
    - Ex. Use Dev environments for 40 hours a week, then shut down instances to save costs

- **Measure Overall Efficiency**
  - Measure the business value delivered from the workload, and the costs associated with running it
    - Use this measure to know the gains made by increasing output and reducing costs

- **Stop Spending Money on Undifferentiated Heavy Lifting**
  - No need to manage data center operations (ex. racking, stacking, maintaining servers)
  - Allows you to focus on business projects rather than IT infrastructure

- **Analyze and Attribute Expenditure**
  - In the cloud it's east to identify and attribute usage & system costs to owners
  - Ex. Cost Allocation Tags

## 5P - Security
### Design Principles
- **Implement Strong Security Foundation**
  - Principle of Least Privilege
  - Eliminate reliance on long-term static credentials

- **Enable Traceability**
  - Monitor, alert, and audit actions and changes to your environment in real-time
    - Ex. CloudTrail, AWS Config

- **Apply Security at All Layers**
  - Ex. NACL, Security Groups, Load Balancers, Application Code, Server

- **Automate Security Best Practices**
  - Allows security mechanisms to scale rapidly and cost-effectively
  - Define and manage security architectures and controls as code in templates

- **Protect Data in Transit and at Rest**
  - Ex. SSL/TLS (Transit)
  - Ex. AWS Managed keys, KMS, CloudHSM (Rest)

- **Keep People Away from Data**
  - Use cloud services to reduce or eliminate the need for data direct access
    - Ex. Encryption, SSM Parameter Store, Secrets Manager

- **Prepare For Security Events**
  - Run incident response simulations
  - Use automated tools to increase detection speed and recovery
