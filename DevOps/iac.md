# **Infrastructure as Code**

# Sections
- [**Infrastructure as Code**](#infrastructure-as-code)
- [Sections](#sections)
- [Overview](#overview)
- [The Infrastructure Resource Lifecycle](#the-infrastructure-resource-lifecycle)
- [Resource Provisioning](#resource-provisioning)
  - [AWS CloudFormation](#aws-cloudformation)
    - [Template Anatomy](#template-anatomy)
    - [Change Sets](#change-sets)
    - [Reusable Templates](#reusable-templates)
    - [Template Linting](#template-linting)
    - [Summary](#summary)
- [Configuration Management](#configuration-management)
- [Monitoring & Performance](#monitoring--performance)
- [Compliance & Governance](#compliance--governance)
- [Resource Optimization](#resource-optimization)
- [Conclusion](#conclusion)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf)

This summary is based off of the July 2017 revision of the **Infrastructure as Code** whitepaper. **Infrastructure as Code (IaC)** is the practice of defining and automating the provisioning of IT infrastructure (compute, network, storage, etc.) through configuration files, in a similar manner to how a software application is constructed by its application code. Rather than relying on manually performed steps, which are prone to human error, lack of agility, and excessive time spent, IaC uses automation to set up infrastructure more quickly and consistently each time.

IaC falls under the larger umbrella of [DevOps](./introduction-to-devops.md) practices, which are used to foster increased velocity in the software development lifecycle (SDLC). This whitepaper focuses on best practices for IaC, the benefits of IaC, and the various AWS services that can leverage IaC for managing cloud infrastructure.

# The Infrastructure Resource Lifecycle
IaC does not only apply to provisioning IT infrastructure resources, but can be used throughout the resource's lifecycle. There are five key, cyclic stages for IT resources, and each stage can leverage IaC: 

1. **Resource Provisioning**
2. **Configuration Management**
3. **Monitoring & Performance**
4. **Compliance & Governance**
5. **Resource Optimization**

The table below summarizes each stage and provides AWS services that can support each stage. Each stage and how the services are used will be explained in the following sections of this document.

<html>
<table>
  <tr>
    <td width="200"><b>Stage</td>
    <td width="400"><b>Description</td>
    <td width="200"><b>AWS Services</td>
  </tr>
  <tr>
    <td><b>Resource Provisioning</td>
    <td>Administrators provision resources according to the specifications they want.</td>
    <td>AWS CloudFormation</td>
  </tr>
  <tr>
    <td><b>Configuration Management</td>
    <td>The resources become components of a management system that supports activities such as patching and tuning.</td>
    <td>AWS Systems Manager<br>AWS OpsWorks</td>
  </tr>
  <tr>
    <td><b>Monitoring & Performance</td>
    <td>Monitoring tools validate the operational status of the resources by examining items such as metrics, log files, etc.</td>
    <td>Amazon CloudWatch</td>
  </tr>
  <tr>
    <td><b>Compliance & Governance</td>
    <td>Compliance frameworks drive additional validation to ensure alignment with corporate and industry standards, as well as regulatory requirements.</td>
    <td>AWS Config</td>
  </tr>
  <tr>
    <td><b>Resource Optimization</td>
    <td>Administrators review performance data and identify changes needed to optimize the environment around criteria such as performance and cost management.</td>
    <td>AWS Trusted Advisor</td>
  </tr>
</table>
</html>

# Resource Provisioning
Administrators can use IaC to for a streamlined, repeatable process for instantiating resources consistently. The following sitauations are an example for where this would be useful:
- A release manager needs to build a replica of a cloud-based production environment for disaster recovery purposes
- A service has to meet certain industry protection standards, and so requires that its infrastructure is configured with a set of security controls each time the service is installed
- The students in a university class each need an environment that contains the appropriate tools for their studies in the course.

 To address these situations and needs, AWS offers [AWS CloudFormation](https://aws.amazon.com/cloudformation/).

## AWS CloudFormation
AWS CloudFormation gives developers and administrators an easy way to create, manage, provision, and update a collection of related AWS resources in an orderly and predicatble way. AWS CloudFormation uses template files to describe *stacks*, a term used for a collection of AWS resources as a single unit. Template files can be used to create identical copies of the stack within and across regions. Once created, stacks can be modified and updated in a controlled and predictable way just by updating their underlying templates. If a stack is deleted, all of its AWS resources are deleted (by default) as well, facilitating easy management. 

In the diagram below, a template file is used to build a stack consisting of a VPC, subnets, security groups, EC2 instances, RDS databases, and a load balancer.

![StackDiagram](../Diagrams/InfrastructureAsCode.png)

### Template Anatomy
CloudFormation templates can be written in JSON or YAML format, and contain parameters, resource declarations, outputs, among other configurations. Templates can also reference other templates, enabling modularization.

The example template below is YAML-formatted. It creates a VPC and an internet gateway, and attaches the internet gateway to the VPC.
```
Resources:
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      InstanceTenancy: default

  IGW:
    Type: AWS::EC2::InternetGateway

  IGWAttachment:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      InternetGatewayId: !Ref IGW
      VpcId: !Ref VPC
```

The `Resources` section is the only required section in a CloudFormation template, but to learn about all of the optional sections, read the [*Template Anatomy* section in the *AWS CloudFormation User Guide*](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html#template-anatomy-sections).

### Change Sets
CloudFormation stacks can be modified by updating their templates, to add, modify, or delete resources. The [change sets feature](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) enables you to preview proposed changes to a stack without actually performing them to see their impact. There are 3 phases to applying a change set to a stack:
1. Create the change set.
   - Submit the changes to the template or parameters of a stack. 
2. Preview the change set.
   - CloudFormation will provide a summary and detailed list of the changes that will occur. Changes may have downstream changes on other resources in the stack, and those will be displayed here.
3. Execute the change set.
   - Once satisfied, you can execute the change set to modify the stack. 

### Reusable Templates
CloudFormation allows for the grouping of stacks logically by function. Instead of grouping all resources in a single template, it is possible to use [nested stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-stack.html) and [cross-stack references](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/walkthrough-crossstackref.html) to reuse and share templates across multiple stacks.

The nested stacks feature allows for creating a child stack as a *resource* within the template of another stack. This means that whenever the parent stack is created, the child stack is created. This can be useful in, for example, sharing infrastructure code across projects, while still maintaining independent stacks for each project.

Cross-stack references allow for stacks to export values that other stacks can then import and use. This can allow for sharing a single resource across multiple projects (for example, a VPC).

### Template Linting
As with application code, CloudFormation templates also should go through some form of static analysis (*linting*) to ensure that the template is syntactically correct and adheres to style guidelines. CloudFormation provides the [ValidateTemplate](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_ValidateTemplate.html) API for this purpose. If the validation check fails, CloudFormation will return a template validation error.

### Summary
The resource lifecycle starts with resource provisioning, and CloudFormation provides a template-based way to achieve this purpose with IaC, just like with application code.

# Configuration Management

# Monitoring & Performance

# Compliance & Governance

# Resource Optimization

# Conclusion

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf)
- [What is DevOps? (AWS)](https://aws.amazon.com/devops/what-is-devops/)
- [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
