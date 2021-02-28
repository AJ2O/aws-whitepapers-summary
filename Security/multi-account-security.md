# **AWS Multi-Account Security Strategy**

# Sections
- [**AWS Multi-Account Security Strategy**](#aws-multi-account-security-strategy)
- [Sections](#sections)
- [Overview](#overview)
- [General Best Practices](#general-best-practices)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdf)

This summary is based off of the June 2017 revision of the **AWS Multi-Account Security Strategy** whitepaper. This document answers the question:
*How do I manage multiple AWS accounts for security purposes?* at a high-level for various situations. It assumes that the reader has basic knowledge of AWS accounts, IAM, S3, CloudTrail, AWS Organizations, and identity federation.

# General Best Practices
- **Clearly define an AWS account-creation process**
  - Maintain an inventory of who is creating AWS accounts, and what each account is used for
- **Define a company-wide AWS usage policy**
  - Empowers customers to leverage AWS without internal confusion about what is or what isn't within policy guidelines
- **Create a security account structure for managing multiple accounts**
  - Makes it easier for companies to:
    - Assess the security of AWS-based deployments
    - Centralize security monitoring and management
    - Manage identity and access
- **Leverage AWS APIs and scripts**
  - Use the AWS APIs in custom scripts to automate the application of baseline configurations across AWS accounts
  - [AWS Control Tower](https://aws.amazon.com/controltower/) is tailored for this purpose, and is the easiest way to set up a secure multi-account AWS environment

# References
- [Whitepaper](https://d1.awsstatic.com/aws-answers/AWS_Multi_Account_Security_Strategy.pdf)
