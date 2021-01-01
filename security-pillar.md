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

# Detection

# Infrastructure Protection

# Data Protection

# Incident Response

# Conclusion

# References
- [Whitepaper (PDF)](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/wellarchitected-security-pillar.pdf) - 37 Pages
- [Whitepaper (Documentation Format)](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)
