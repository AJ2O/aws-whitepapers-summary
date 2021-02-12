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
- Find and address bugs quicker
- Reduce the time needed to validate new software updates
- Improve software quality

### Services
- [**CodeCommit**](https://aws.amazon.com/codecommit/) - secure & scalable source control service that hosts private git repositories
- [**CodeBuild**](https://aws.amazon.com/codebuild) - fully managed CI service that can compile code, run tests, and produce software packages ready for deployment
  - Can use GitHub, BitBucket, CodeCommit, or S3 as the source code provider
- [**CodeArtifact**](https://aws.amazon.com/codeartifact) - fully managed artifact repository service that can be used to store, publish, and share software packages
  - Works with common package managers and tools such as Maven, Gradle, yarn, pip, etc.


# Continuous Delivery

### Definition

**Continuous Delivery (CD)** is a software development practice that expands upon CI, by deploying the code changes to a testing and/or production environment after a successful build stage.

### Benefits
- Ensures all deployment-ready build artifacts have passed through a standardized validation process
- A test environment allows for detailed verification of software (beyond unit testing) before deploying it to customers
  - Examples: UI testing, load testing, integration testing

### Services
- [**CodeDeploy**](https://aws.amazon.com/codedeploy) - automated deployment service capable of deploying software to:
  - [Amazon EC2 Instances](https://aws.amazon.com/ec2)
  - [AWS Fargate Containers](https://aws.amazon.com/fargate)
  - [AWS Lambda Functions](https://aws.amazon.com/lambda/)
  - On-Premises servers
- [**CodePipeline**](https://aws.amazon.com/codepipeline) - continuous delivery service that enables modelling and visualizing the automated steps to release software. The steps below can be ordered, enumerated, and integrated with each other in granular detail:
  - **Source Control:** CodeCommit, S3, and popular tools (GitHub, BitBucket, etc...)
  - **Build:** CodeBuild, Jenkins and other third-party tools
  - **Test:** CodeBuild, BlazeMeter, and other third-party tools
  - **Deploy:** CodeDeploy, [CloudFormation](https://aws.amazon.com/cloudformation/), [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/), and many other AWS services
  - **Invoke:** Lambda and [Step Functions](https://aws.amazon.com/step-functions/) can be invoked during and/or between stages

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
