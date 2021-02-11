# **Introduction to DevOps on AWS**

# Overview
- [Source](https://docs.aws.amazon.com/whitepapers/latest/introduction-devops-aws/welcome.html)

This summary is based off of the October 2020 revision of the Introduction to DevOps on AWS whitepaper.

**DevOps** is the combination of cultural and practical practices, as well as tools that increase the speed and stability of software development in an organization. The essential practices associated with DevOps are the following:
- [Continuous Integration](continuous-integration)
- [Continuous Delivery](#continuous-delivery)
- [Infrastructure as Code](#infrastructure-as-code)
- [Monitoring and Logging](#monitoring-and-logging)
- [Communication and Collaboration](#communication-and-collaboration)
- [Security](#security)

This whitepaper highlights how AWS can help an organization achieve the aforementioned practices.

# Continuous Integration

### Definition
**Continuous Integration (CI)** is a software development practice where code developers regularly merge their code into a central code repository, which then automatically triggers  builds and executes tests against the changed code.

<img src="Diagrams/ContinuousIntegration.png" alt="CI"/>

### Benefits 
- find and address bugs quicker
- reduce the time needed to validate new software updates
- improve software quality

### Services
- [**CodeCommit**](https://aws.amazon.com/codecommit/) - secure & scalable source control service that hosts private git repositories
- [**CodeBuild**](https://aws.amazon.com/codebuild) - fully managed CI service that can compile code, run tests, and produce software packages ready for deployment
  - Can use GitHub, BitBucket, CodeCommit, or S3 as the source code provider
- [**CodeArtifact**](https://aws.amazon.com/codeartifact) - fully managed artifact repository service that can be used to store, publish, and share software packages
  - Works with common package managers and tools such as Maven, Gradle, yarn, pip, etc.


# Continuous Delivery

### Definition

**Continuous Delivery (CD)** is a softeare development practice

### Benefits

### Services

# Deployment Strategies


# Infrastructure as Code

### Definition

### Benefits

### Services

# Automation


# Monitoring and Logging

### Definition

### Benefits

### Services


# Communication and Collaboration

### Definition

### Benefits

### Services


# Security

### Definition

### Benefits

### Services

# Conclusion

# References
