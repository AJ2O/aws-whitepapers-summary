# **AWS Well-Architected Framework - Security Pillar**

# Overview
- [Source Whitepaper](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/wellarchitected-security-pillar.pdf) - 37 Pages
- [Documentation Format](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

This summary is based off of the July 2020 revision of the AWS Well-Architected Framework - Security Pillar whitepaper.

This whitepaper describes AWS recommendations and strategies for designing cloud architectures with security in mind. Security in the cloud is composed of five areas:
1. Identity and Access Management
2. Detection
3. Infrastructure Protection
4. Data Protection
5. Incident Response

# Security Foundations

## Design Principles
- **Implement a Strong Identity Foundation**
  - Implement the principle of least privilege
  - Centralize identity management
- **Enable Traceability**
  - Monitor, alert, and audit actions in your environment
- **Apply Security at all Layers**
  - Apply a defense in depth approach
    - Ex. VPC, load balancing, instance, operating system, application
- **Automate Security Best Practices**
  - Automated software security mechanisms improve your ability to securely scale more rapidly and cost-effectively
- **Protect Data in Transit and at Rest**
  - Use encryption, tokenization, and access control where appropriate
- **Keep People Away from Data**
  - Reduce or eliminate the need for direct access or manual processing of data
- **Prepare for Security Events**
  - Create incident management processes and investigation policies that align with your organizational requirements

## Operating Your Workload Securely
- **Use a Threat Model to Identify and Prioritize Risks**
  - Maintain an up-to-date register of potential threats
- **Identify and Validate Control Objectives**
  - Ongoing validation helps you measure the effectiveness of risk mitigation
- **Keep up to date with Security Threats**
  - Helps you recognize new attack vectors
- **Keep up to date with Security Recommendations**
  - Using AWS and industry security recommendations
- **Evaluate and Implement New Security Services and Features Regularly**
  - Evolves the security posture of your workload
- **Automate Testing and Validation of Security Controls in Pipelines**
  - Establish secure baselines for your security mechanisms in build/pipeline processes, then use tools and automation to test and validate all security controls continuously

## AWS Account Management and Separation
AWS recommends to organize workloads in separate accounts and group accounts based on function, compliance or a common set of controls.

- **Separate Workloads using Accounts**
  - Strongly recommended for isolating production environments from development and test environments
  - Can also be used as a boundary for workloads that processes data of different sensitivity levels
- **Secure AWS Account**
  - Different aspects:
    - Securing the root user
    - Don't use the root user
    - Keep contact info up to date
- **Manage Accounts Centrally**
  - [AWS Organizations](https://aws.amazon.com/organizations/) automates AWS account creation and management, and control of those accounts after they are created
- **Set Controls Centrally**
  - Control what your AWS accounts can do by only allowing specific services, regions, and service actions at the appropriate level
- **Configure Services and Resources Centrally**
  - AWS Organizations helps you configure AWS services that apply to all of your accounts
  - Examples:
    - Central logging of all actions across all accounts using [AWS CloudTrail](https://aws.amazon.com/cloudtrail/)
    - Audit workloads for compliance across all accounts using [AWS Config](https://aws.amazon.com/config/)

# Identity and Access Management

## Identity Management
There are two types of identities in AWS. **Human Identities** include administrators, operators, developers, consumers, etc. **Machine Identities** include applications, operational tools, and components. Identity management recommendations by AWS are listed below:
- **Rely on a Centralized Identity Provider**
  - For federations with indiviual AWS accounts, you can use centralized identities with a [SAML 2.0](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)-based provider
  - For federation to multiple accounts in your AWS Organization, you can configure your identity source in [AWS SSO](http://aws.amazon.com/single-sign-on/) to specify where your users and groups are stored
  - For managing consumers of your applications, such as for a mobile app, you can use [Amazon Cognito](http://aws.amazon.com/cognito/) and sign users in through a third party such as Amazon, Facebook or Google
- **Leverage User Groups and Attributes**
  - You can assign users to a group, and assign permissions (through attached policies) to the group to apply to all its members
- **Use Strong Sign-In Mechanisms**
  - You can specify a complex password policy or enforce MFA
- **Audit and Rotate Credentials Periodically**
- **Store and Use Secrets Securely**

## Permissions Management

- **Define Permission Guardrails of your Organization**
  - Using AWS Organizations, you can use service control policies to apply allow or deny permissions at the account-level and organization unit-levels
- **Grant Least Privilege Access**
  - Ensures identities are only permitted to perform the minimal set of functions necessary to fulfill a specific task
- **Analyze Public and Cross Account Access**
- **Share Resources Securely**
- **Establish Emergency Access Process**

# Detection

## Configure
- **Configure Service and Application Logging**
  - At the account level:
    - [AWS CloudTrail](https://aws.amazon.com/cloudtrail/) - event history of AWS API calls
    - [AWS Config](https://aws.amazon.com/config/) - monitor and record AWS resource configurations
    - [AWS GuardDuty](https://aws.amazon.com/guardduty/) - threat detection service that monitors for malicious activity
    - [AWS Security Hub](http://aws.amazon.com/security-hub) - aggregates, organizes, and prioritizes security alerts or findings from multiple AWS services
  - Many core AWS services provide service-level logging features
- **Analyze Logs, Findings, and Metrics Centrally**
  - Deeply integrate the flow of security events and findings into a notification and workflow system
    - Ex. ticketing system, bug/issue system
    - Allows you to route, escalate, and manage events or findings

## Investigate
- **Implement Actionable Security Events**
  - Establish a [runbook](https://wa.aws.amazon.com/wat.concept.runbook.en.html) for each detective mechanism you have
- **Automate Response Events**
  - Can be achieved using [Amazon EventBridge](http://aws.amazon.com/eventbridge)

# Infrastructure Protection

## Protecting Networks


## Protecting Compute


# Data Protection

## Data Classification


## Protecting Data at Rest


## Protecting Data in Transit


# Incident Response

# Conclusion

# References
- [Whitepaper (PDF)](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/wellarchitected-security-pillar.pdf)
- [Whitepaper (Documentation Format)](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)
