# **Cloud Native Security Whitepaper**

# Overview
- [Source](https://lists.cncf.io/g/cncf-toc/attachment/5509/0/CNCF_cloud-native-security-whitepaper-Nov2020.pdf)

This summary is based off of the November 2020 revision of the [Cloud Native Computing Foundation - Cloud Native Security Whitepaper](https://lists.cncf.io/g/cncf-toc/attachment/5509/0/CNCF_cloud-native-security-whitepaper-Nov2020.pdf).

Patterns of development and deployment in the technology industry have shifted towards becoming "[cloud native](https://github.com/cncf/toc/blob/main/DEFINITION.md)", with adoptions to modern workflows such as [DevOps](https://aws.amazon.com/devops/what-is-devops/) and [Agile](https://www.atlassian.com/agile). These processes have a focus on rapid development and deployment, requiring a need for security to meet the scale needs of these dynamic workloads in the cloud.

Cloud native development can be modelled in the distinct phases that comprise the application lifecycle, and security must be ensured at each phase:
- **Develop**
  - The process of writing and managing the application source code in a revision system
- **Distribute**
  - The process of building code and producing artifacts ready for deployment
- **Deploy**
  - The process of transferring artifacts to an environment, and starting the application
- **Runtime**
  - The environment that contains the running application
    - Ex. Hardware, host, OS, container image runtime
  - In a [microservices architecture](https://aws.amazon.com/microservices/), an application may be distributed across multiple runtime environments

Cloud native security seeks to ensure the same levels of diligence, integrity, trust, and threat prevention as traditional security models, while integrating modern concepts of ephemerality, distribution, and immutability.

# Cloud Native Layers

## Lifecycle
A cloud native application's **lifecycle** refers to the technology, practices, and processes that enable workloads to run within the context of a cloud environment. It is comprised of four continuous phases: Develop, Distribute, Deploy, and Runtime. The phases below describe methods of implementing security at each of these phases.

## Develop
- **Security Checks in Development**
  - Tools to perform the scanning of known vulnerabilities or high risk configurations in  [Infrastructure as Code (IaC)](https://stackify.com/what-is-infrastructure-as-code-how-it-works-best-practices-tutorials/) templates or  application manifests in the IDE or during a Pull Request
  - Use dedicated development, testing, and production environments to provide isolated environments for systems, applications, and golden VM/container base images
- **Development of Tests**
  - Tests built for code should be business-critical, have a high-threat profile, subject to frequent change, or have a historical source of bugs
  - This can identify high-risk and high-impact code hotspots that provide a high return on investment for developing tests

## Distribute
- **Build Pipeline** - [Continuous Integration (CI)](https://aws.amazon.com/devops/continuous-integration) servers should be isolated and restricted to projects of a similar security sensitivity
- **Image Scanning** - Implement scanning in the CI pipeline for vulnerabilities and potential malware in open source packages or images from public repositories
- **Dynamic Analysis** - Scan deployed infrastructure for [configuration drift](https://www.continuitysoftware.com/blog/it-resilience/what-is-configuration-drift/) and validation of the expected network attack surface
- **Signing, Trust, and Integrity** - Signing of image content at build time, and validation of the signed data before use protects that image data from tampering between build and runtime
- **Encryption** - Ensures that image contents remain confidential for promotion from build time through runtime

## Deploy
- **Pre-Flight Deployment Checks** - Verify the existence, applicability, and current state of:
  - Image signature and integrity
  - Image runtime policies
  - Container runtime policies
  - Host vulnerability and compliance controls
- **Response & Investigation** - An application should provide logs regarding authentication, authorization, actions and failures
  - These elements provide a trail of evidence to follow when an investigation takes place and a root cause needs to be established

## Runtime Environment
The Runtime phase is comprised of three main areas: [compute](#compute), [storage](#storage), and [access](#access).

### Compute
- **Containers**
  - Containers provide virtualization on the OS-level, so using an underlying read-only OS is important, with other services disabled
  - Improperly configured containers can impact the host kernel security, and thus compromising all other services running on the same host
- **Orchestration**
  - Orchestration systems have many components that co-exist, yet can operate independently of each other, and thus present attack vectors that impact the system's overall security:
    - Malicious access and misuse of the orchestrator's APIs
    - Interception of control plane or application traffic
    - Unauthorized changes to any of the system's initial configurations
  - Orchestrator control plane components (ex. scheduler, API server, controller-manager) should communicate via mutual authentication
- **Serverless Functions**
  - Ingress network inspection can be used to detect and remove malicious payloads and commands to functions (ex. SQL injection)
  - Egress network inspection can be used to detect and if possible, prevent access to functions from [command and control (C&C)](https://www.trendmicro.com/vinfo/us/security/definition/command-and-control-server) servers
  - Using principle of least privilege, internal processes must only execute functions defined in an explicit allow list
- **Audit Log Analysis**
  - Audit logs can be used to identify system compromise, abuse, or misconfiguration
  - Automation can be used to detect system violations based on preset rules that filter violations based on the organization's policies
  - Logs should be immediately routed to a location inaccessible using  cluster-level credentials, to prevent attackers from covering their tracks

### Storage
- **Types of Storage**
  - *Presented Storage* - Storage directly made available to workloads such as:
    - Volumes
    - Block stores
    - Shared Filesystems
  - *Access Storage* - Storage accessed via APIs:
    - Object stores
    - Key-Value stores
    - Databases
    - Caching layers
  - The interfaces by which workloads can access, store, and consume storage may be protected via access controls, authentication, authorization, encryption at rest, and encryption in transit

### Access
- **Identity and Access Management (IAM)**
  - Workloads should be explicitly authorized to communicate with each other using mutual authentication
- **Credential Management**
  - *Hardware Security Modules (HSMs)* - HSMs should be used to physically protect cryptographic secrets with an encryption key that does not leave the HSM
  - *Credential Management Cycle* - Secrets, whenever possible, should have a short expiration period, after which they become useless
- **Availability** - Implement guardrails for DoS and DDoS attacks

# Security Assurance

## Security vs. Compliance
- **Security** is a risk management process that seeks to identify and address risks posed to a system
  - The iterative hardening of systems will reduce or transfer risk depending on an application's or organization's risk profiles and tolerances
  - Ex. For updating a base image, the following *security* considerations are made:
    - Which additional ports should be open?
    - How should we review updated packages used by this image?
    - What permissions should be made available?
- **Compliance standards** form requirement definitions by which systems are assessed against
  - The outcomes of the assessment are either a pass or a fail
- A compliant system is not guaranteed to be secure, nor a secure system guaranteed to be compliant

## Threat Modelling
- **End-to-end Architecture** - A clear understanding of the organization's cloud architecture should result in data impact guidance and classifications
- **Threat Identification** - It is recommended to use a mature, well-used model of threats such as [STRIDE](https://www.microsoft.com/security/blog/2007/09/11/stride-chart/). Common threat examples are listed below:
  - **S**poofing - a cluster admin by stealing credentials via social engineering
  - **T**ampering - of an API config file
  - **R**epudiation - of an attacker's actions because of disabled API auditing
  - **I**nformation Disclosure - if an attacker compromises a running workload and exfiltrates data
  - **D**oS - a pod doesn't have applied resource limits, and the node's CPU and memory and fully consumed
  - **E**levation of Privilege - a pod is running with unrestricted permissions

## Security Stack
- **Compute & Node Checks** - organizations should leverage tooling that assert hardening and security of compute resources before they are ready to accept workloads
- **Workload & Host Runtime Security** - there are four key protection surface areas to consider:
  - Process / Container / System Level Security
  - Network Security
  - Data Security
  - Application Security
- **Zero Trust Architecture**
- **Least Privilege** - not only applied to users performing various functions operating the environment, but it also to every level of the workload's architecture
  - Rootless services and containers are vital to ensure that if an attacker does break into the environment, they cannot easily access the underlying host or other containers

# Compliance

# References
- [Source Whitepaper](https://lists.cncf.io/g/cncf-toc/attachment/5509/0/CNCF_cloud-native-security-whitepaper-Nov2020.pdf)