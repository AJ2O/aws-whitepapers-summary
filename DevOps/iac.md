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
  - [AWS Systems Manager](#aws-systems-manager)
    - [Document Structure](#document-structure)
    - [Best Practices](#best-practices)
      - [Run Command](#run-command)
      - [Automation](#automation)
      - [State Manager](#state-manager)
      - [Inventory](#inventory)
  - [AWS OpsWorks](#aws-opsworks)
    - [Best Practices](#best-practices-1)
  - [Summary](#summary-1)
- [Monitoring & Performance](#monitoring--performance)
  - [Amazon CloudWatch](#amazon-cloudwatch)
  - [Amazon CloudWatch Logs](#amazon-cloudwatch-logs)
  - [Amazon CloudWatch Events / Amazon EventBridge](#amazon-cloudwatch-events--amazon-eventbridge)
  - [Best Practices](#best-practices-2)
  - [Summary](#summary-2)
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
    <td>Amazon CloudWatch<br>Amazon EventBridge</td>
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
- A release manager needs to build a replica of a cloud-based production environment for disaster recovery purposes.
- A service has to meet certain industry protection standards, and so requires that its infrastructure is configured with a set of security controls each time the service is installed.
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
Once the infrastructure is provisioned and running, you need to manage the ongoing configuration needs of its environment. These are some examples where this would be useful:
- A release manager wants to deploy an application across a group of servers and perform a rollback if any problems occur.
- A system administrator is tasked with installing a new operating system package in all developer environments, but the other environments must remain untouched.
- An application administrator needs to periodically update a configuration file across all application servers.

One approach to address this need is with *infrastructure immutability*, where the existing resources are taken down, and a new infrastructure with new configuration is provisioned in its place.

If environments have high levels of durability however, it's much faster to make incremental changes instead of reprovisioning. AWS offers [AWS Systems Manager](https://aws.amazon.com/systems-manager/) and [AWS OpsWorks](https://aws.amazon.com/opsworks/) for these purposes.

## AWS Systems Manager
AWS Systems Manager (SSM) is a collection of tools and services that allow for the visibility and control of your AWS infrastructure, all under a unified interface. For configuration management, it helps you understand and control your AWS environment, EC2 instances, and even on-premises servers. You can track and remotely manage systems, OS patch levels, application configurations, and other functionalities across fleets of servers. These capabilities help with automating complex and repetitive tasks, preventing drift, and maintaining software compliance across all your systems.

The table below lists some of the configuration management capabilities of SSM, and examples for each.

<html>
<table>
  <tr>
    <th width="160">Task</th>
    <th width="300">Description</th>
    <th width="300">Example</th>
  </tr>
  <tr>
    <th>Run Command</th>
    <td>Run commands on EC2 instances without SSH or RDP.</td>
    <td>Run a shell script on 50 instances at one time.</td>
  </tr>
  <tr>
    <th>Automation</th>
    <td>Automate routine maintenance tasks and scripts.</td>
    <td>Stop developer instances on Friday evenings, and start them up again on Monday morning.</td>
  </tr>
  <tr>
    <th>Patch Manager</th>
    <td>Automates the process of patching instances for updates.</td>
    <td>Keep a fleet of servers at the same patch level for security.</td>
  </tr>
  <tr>
    <th>State Manager</th>
    <td>Creates states that represent a certain configuration applied to instances.</td>
    <td>Keep track of which instances have been updated to the current, stable version of Apache HTTP.</td>
  </tr>
  <tr>
    <th>Maintenance Windows</th>
    <td>Define schedules to apply patches, updates, scripts, and other tasks to instances.</td>
    <td>Run security patches every Sunday between 0:00 AM - 2:00 AM.</td>
  </tr>
  <tr>
    <th>Inventory</th>
    <td>Collect OS, application, and instance metadata.</td>
    <td>Determine what versions of Apache HTTP the servers currently have.</td>
  </tr>
</table>
</html>

To read about all the SSM features, read the [*Systems Manager Capabilities* section in the AWS Systems Manager User Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/features.html).

### Document Structure
An SSM document defines the actions that SSM performs on its managed instances. There are already numerous pre-configured SSM documents provided by AWS, and you can create custom documents as well. Documents can also be version-controlled and shared across AWS accounts.

Documents can be written in JSON or YAML format, and include steps and parameters that you specify. The following is an example of a document for a Windows-based host with two steps, and no parameters. It uses PowerShell to run the `ipconfig` command to gather network configuration of the instance, and then installs MySQL.

```
{
 "schemaVersion": "2.0",
 "description": "Sample version 2.0 document v2",
 "parameters": {},
 "mainSteps": [
    {
      "action": "aws:runPowerShellScript",
      "name": "runShellScript",
      "inputs": {
        "runCommand": ["ipconfig"]
      }
    },
    {
      "action": "aws:applications",
      "name": "installapp",
      "inputs": {
        "action": "Install",
        "source":
        "http://dev.mysql.com/get/Downloads/MySQLInstaller/mysqlinstaller-community-5.6.22.0.msi"
      }
    }
  ]
}
```

### Best Practices
Below are some best practices for some of SSM's capabilities:

#### Run Command
- Improve your security posture by leveraging Run Command to access your EC2 instances, instead of SSH/RDP.
- Audit all API calls made by or on behalf of Run Command using AWS CloudTrail.
- Use the rate control feature in Run Command to perform a [staged command execution](https://docs.aws.amazon.com/systems-manager/latest/userguide/send-commands-multiple.html#send-commands-rate).

#### Automation
- Use Automation to simplify creating AMIs from the AWS Marketplace or custom AMIs, using public documents or by creating your own.
- Use the documents `AWS-UpdateLinuxAmi` or `AWS-UpdateWindowsAmi` or create a custom Automation document to build and maintain images.

#### State Manager
- Update the SSM agent periodically using the preconfigured `AWS-UpdateSSMAgent` document
- Use tags to create application groups. Then target instances using the `Targets` parameter, instead of specifying individual instance IDs.
- Use a centralized configuration repository for all your SSM documents, and share documents across the organization.

#### Inventory
- Use Inventory in combination with AWS Config to audit your application configuration overtime.

## AWS OpsWorks
AWS OpsWorks is a configuration management service that provides fully managed instances of [Chef Automate](https://www.chef.io/products/chef-automate) and [Puppet Enterprise](https://puppet.com/products/puppet-enterprise/). OpsWorks is also offered as [AWS OpsWorks Stacks](https://aws.amazon.com/opsworks/stacks/), which lets you manage application stacks using Chef recipes.

Known as recipes in Chef, and modules in puppet, these tools use configuration files that are analagous to CloudFormation templates. They are a form of source code that can be version-controlled, extending the principle of IaC to the configuration management stage of the resource lifecycle.

Below is a sample Chef recipe used in OpsWorks Stacks. It checks the EC2 instance's operating system, and runs the appropriate command to install the `tree` package if applicable.

```
instance = search("aws_opsworks_instance").first
os = instance["os"]

if os == "Red Hat Enterprise Linux 7"
    Chef::Log.info("********** Operating system is Red Hat Enterprise Linux. **********")
elsif os == "Ubuntu 12.04 LTS" || os == "Ubuntu 14.04 LTS" || os == "Ubuntu 16.04 LTS" || os == "Ubuntu 18.04 LTS"
    Chef::Log.info("********** Operating system is Ubuntu. **********") 
elsif os == "Microsoft Windows Server 2012 R2 Base"
    Chef::Log.info("********** Operating system is Windows. **********")
elsif os == "Amazon Linux 2018.03" || os == "Amazon Linux 2"
    Chef::Log.info("********** Operating system is Amazon Linux. **********")
elsif os == "CentOS Linux 7"
    Chef::Log.info("********** Operating system is CentOS 7. **********")
else
    Chef::Log.info("********** Cannot determine operating system. **********")
end

case os
when "Ubuntu 12.04 LTS", "Ubuntu 14.04 LTS", "Ubuntu 16.04 LTS", "Ubuntu 18.04 LTS"
  apt_package "Install a package with apt-get" do
    package_name "tree"
  end
when "Amazon Linux 2018.03", "Amazon Linux 2", "Red Hat Enterprise Linux 7", "CentOS Linux 7"
  yum_package "Install a package with yum" do
    package_name "tree"
  end
else
  Chef::Log.info("********** Cannot determine operating system type, or operating system is not Linux. Package not installed. **********")
end
```

To experiment with OpsWorks Stacks in particular, try out the tutorial [*Getting Started with Cookbooks in AWS OpsWorks Stacks*](https://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-cookbooks.html).

This document will not go into the details of using Chef or Puppet, as they are widely used and heavily documented, but know that these configuration management tools exist and that AWS provides managed instances of each, so that you don't have to configure a Chef or Puppet server from scratch.

### Best Practices
- Consider storing your Chef recipes and Puppet modules in a source control repository, just as you would with application code.
- Use IAM to limit access to OpsWorks API calls.

## Summary
AWS Systems Manager lets you deploy, customize, enforce, and audit an expected state configuration for your servers. AWS OpsWorks enables you to use Chef or Puppet to support the configuration of an environment after it's been provisioned. SSM documents, Chef recipes, and Puppet modules can become part of the infrastructure code base and be version-controlled, just like application source code.

# Monitoring & Performance
It's important to capture key metrics to assess the health of the environment and take corrective action when problems arise. Metrics provide visibility into the environment's state, and can allow your organization to automatically respond to events. To address the need to collect and analyze metrics and log files, AWS offers the [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) service.

CloudWatch is a set of services that ingests, interprets, and responds to runtime metrics, logs, and events. It automatically aggregates metrics from resources across AWS services, and can be configured to respond with built-in actions such as notifications, or with custom actions such as invoking an [AWS Lambda](https://aws.amazon.com/lambda/) function. The code for the Lamdba functions become part of the infrastructure code base, extending IaC to the monitoring level. CloudWatch is composed of three main services: the main CloudWatch service, CloudWatch Logs, and CloudWatch Events. These will now be explored in more detail.

## Amazon CloudWatch
The main CloudWatch service collects and tracks metrics for many AWS services, including EC2, DynamoDB, RDS, API Gateway, etc. Custom metrics can also be created and pushed from your applications to be monitored by CloudWatch. In CloudWatch, you can implement metric-based alarms that can invoke notifications or custom actions such as Lambda functions.

## Amazon CloudWatch Logs
CloudWatch Logs monitors and stores logs from EC2, CloudTrail, Lambda, and other sources. Ingested log data can be the basis for new CloudWatch metrics, that can in turn, trigger your CloudWatch alarms. You can use this capability to monitor and alert on any resource that generates logs, without needing to write additional application code.

## Amazon CloudWatch Events / Amazon EventBridge
CloudWatch Events produces a stream of changes from AWS environments, applies a rules engine, and delivers matching events to specified targets. Example events that can be streamed include changing an EC2 instance's state, taking an EBS snapshot, AWS API calls, AWS Trusted Advisor optimization notifications, and much more. Targets include built-in actions such as SNS notifications, or custom responses such as Lambda functions.

[Amazon EventBridge](https://aws.amazon.com/eventbridge/) is the new implementation of CloudWatch, with more robust functionality. It can use sources from third-party Software-as-a-Service (SaaS) applications, and your own applications. It uses the same API as CloudWatch Events, so all of your existing API usage can remain the same.

The ability of an infrastructure to respond to selected events offers benefits in both operations and security. For operations, events automate maintenance activities without having to manage a separate scheduling system, and for information security, events can can provide notifications of console logins, authentication failures, and risky API calls discovered by CloudTrail.

[Linked here](https://github.com/AJ2O/aws-free-tier) is a solution using Terraform (a third-party IaC tool), Lambda, and EventBridge to keep your AWS account from exceeding free tier. Once deployed, it uses its many rules to detect events and API calls made in your account and respond accordingly. Here are some things it can do automatically at the time of this writing:
- If you launch an EC2 instance with a type outside of free tier (example: `m5.large`), the instance will be terminated.
- If you take an EBS snapshot and your snapshot storage will exceed free tier, the snapshot is deleted.
- If you try to create a DynamoDB table, and the aggregate RCUs and WCUs across all your DynamoDB tables will exceed free tier, the new table will be terminated.

## Best Practices
- Ensure that all AWS resources are emitting metrics.
- Send logs from AWS resources, including S3 and EC2 to CloudWatch Logs for analysis using log stream triggers and Lambda functions.
- Use CloudWatch Events / EventBridge to respond to application-level issues.

## Summary
Monitoring is essential to understand systems behaviour and to automate data-driven reactions. CloudWatch can collect the relevant metrics, EventBridge can alert on environment changes, and Lambda functions can respond to events triggered by the former two services, thereby extending IaC into operations-related activities.

# Compliance & Governance

# Resource Optimization

# Conclusion

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf)
- [What is DevOps? (AWS)](https://aws.amazon.com/devops/what-is-devops/)
- [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
- [Systems Manager Capabilities (AWS User Guide)](https://docs.aws.amazon.com/systems-manager/latest/userguide/features.html)
- [Getting Started with Cookbooks in AWS OpsWorks Stacks (AWS User Guide)](https://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-cookbooks.html)