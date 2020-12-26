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
- **Delegate Advanced Technology Tasks to Cloud Vendor**
  - Ex. Storage (S3), Media Transcoding (Elastic Transcoder), Machine Learning (SageMaker, Lex, Rekognition, etc.) and more, are all managed services
  - Development teams don't need to learn about hosting and running these technologies on servers
    - Can focus on product development

- **Use Serverless Architectures**
  - No need to manage underlying servers, and take advantage of scaling and high availability
    - Ex:
      - Hosting a static website on S3 instead of a server
      - Managed NoSQL database service in DynamoDB
      - CI/CD tools such as CodePipeline

- **Experiment More Often**
  - Cloud resources are virtual and automatable, so comparative testing can be quickly achieved
    - Ex. Quick to compare instance configurations or storage solutions

- **Mechanical Sympathy**
  - Consider how cloud services are used, and select the most appropriate service for your workload

## 5P - Reliability
### Design Principles

- **Automatically Recover From Failure**
  - Monitor workloads for Key Performance Indicators (KPIs), and trigger automation once a threshold is breached
    - KPIs should measure business value, not simply technical aspects of the infrastructure

- **Test Recovery Procedures**
  - In the cloud, you can simulate different failure scenarios, and test recovery procedures
    - Exposes testable failure pathways that can be remediated before a real scenario occurs

- **Scale Horizontally to Increase Workload Availability**
  - Split one large resource into multiple, smaller resources, and distribute workload tasks among them
    - Reduces the impact of a single failure on the overall workload

- **Stop Guessing Capacity**
  - Many cloud services implement auto-scaling and self-healing to maintain the optimal amount of resources without under- or over-provisioning

- **Manage Changes with Automation**
  - Changes to infrastructure should be made using automation
    - Is consistent
    - Can be tracked and reviewed

## 5P - Operational Excellence
### Design Principles
- **Perform Environment Operations as Code**
  - Apply the same discipline for application to your cloud environment
    - The entire workload and environment can be defined and updated as code
    - Ex. CloudFormation
  - Reduces human error, and enables consistency

- **Make Frequent, Small, Reversible Changes**
  - Design workload components to be updated regularly
  - Small changes can be easily reversed if they cause failure

- **Refine Operations Procedures Frequently**
  - As the workload evolves, evolve operation procedures accordingly

- **Anticipate Failure**
  - Regularly test failure scenarios and validate associated response procedures, so teams are familiar with their execution

- **Learn From All Operational Failures**
  - Share what is learned across teams and the organization to drive improvement

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

## 5P - Security
### Design Principles

### Best Practices

