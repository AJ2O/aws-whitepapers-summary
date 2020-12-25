# AWS Well-Architected Framework

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

**Technology Portfolio**
- Collection of workloads required for the business to operate

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

- **Experiment more often**
  - Cloud resources are virtual and automatable, so comparative testing can be quickly achieved
    - Ex. Quick to compare instance configurations or storage solutions

- **Mechanical Sympathy**
  - Consider how cloud services are used, and select the most appropriate service for your workload

### Best Practices

## 5P - Reliability
### Design Principles

- **Automatically Recover From Failure**
  - Monitor workloads for Key Performance Indicators (KPIs), and trigger automation once a threshold is breached
    - KPIs should measure business value, not simply technical aspects

- **Test Recovery Procedures**
  - In the cloud, you can simulate different failure scenarios, and test recovery procedures
    - Exposes testable failure pathways that can be remediated before a real scenario occurs

- **Scale Horizontally to Increase Workload Availability**
  - Split one large resource into multiple, smaller resources
    - Reduces impact of a single failure on the overall workload

- **Stop Guessing Capacity**
  - Many cloud services implement auto-scaling and self-healing to maintain the optimal amount of resources without under- or over-provisioning

- **Manage Changes with Automation**
  - Changes to infrastructure should be made using automation
    - Is consistent
    - Can be tracked and reviewed

### Best Practices

## 5P - Operational Excellence
### Design Principles

### Best Practices

## 5P - Cost Optimization
### Design Principles

### Best Practices

## 5P - Security
### Design Principles

### Best Practices

