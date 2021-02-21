# **Modern Application Development on AWS**

# Overview
- [Source Whitepaper](https://d1.awsstatic.com/whitepapers/modern-application-development-on-aws.pdf) - 41 Pages
- [Documentation Format](https://docs.aws.amazon.com/whitepapers/latest/modern-application-development-on-aws/welcome.html)

This summary is based off of the October 2019 revision of the Modern Application Development on AWS whitepaper.

# Introduction

This whitepaper outlines some best practices and design patterns to help accelerate your organization's innovation on the AWS Cloud. According to the whitepaper, rapid innovation is the key driver to business success, and companies should constantly update their processes of designing, building, and administering their applications to customers. This culture of innovation and continuous change is referred to as **modern application development**. Modern application development is afforded on cloud services, since companies can spend less time administering IT infrastructure, and more time developing their core products.

# Modern Application Development

## Capabilities of Modern Applications
Modern applications should be:
- **Secure** - secure at the application and infrastructure layers
- **Resilient** - failures shouldn't compromise the entire application, but be handled gracefully in code
- **Elastic** - scale out and in depending on traffic
- **Modular** - composed of loosely coupled components, each with a distinct responsibility
- **Automated** - integration and deployment processes of new application releases must be automated
- **Interoperable** - each service exposes required functionality through public APIs, and can be backward compatible

## Best Practices of Modern Application Development
- **Security & Compliance** - authentication (IAM), authorization (role-based control), auditing (CloudTrail, Config), validation (test functionality)
- **Microservice Architecture** - break down a large, monolithic application into loosely-coupled individual services that can work together to deliver the same goal
- **Using Serverless Technology** - no need to spend time administering servers, and you can just focus on developing the core product
- **Automating Deployment with CI/CD** - automate the build/test/deploy process to enable rapid releases in a consistent manner
- **Managing Infrastructure as Code** - model the application with infrastructure as code, and then it can be integrated with CI/CD automation
- **Monitoring and Logging** - create and monitor logs and metrics, then use the data to maintain and improve customer experience

# Modern Application Design Patterns

## Implementing Microservice Architecures using AWS Services

- **API Gateways**
  - Can be used to consolidate calls to different backend services from different client interfaces
  - Grouped under a unified API that integrates with the backend endpoints
- **Service Discovery and Service Registries**
  - Services are separated in a microservice architecture, so a service registry is used as a central reference point for services to discover and communicate with each other
- **Circuit Breaker**
  - Regulate calls between microservices with a circuit breaker
  - If service A makes a call to service B, but service B is delayed or causes an error, the circuit breaker stops routing calls to service B for a period of time
    - Allows time for service B to self-heal and recover
- **Command-Query Responsibility Segregation (CQRS)**
  - Separate the data command (ex. inserts, updates) and query (ex. select) parts of a system
  - Allows for scaling the different functions independently
    - Ex. Perform writes on the regular database, and perform reads on read replicas
- **Event Sourcing**
  - Any events with significance to business logic are added to a durable events log
    - The application's state can be rebuilt by reprocessing the logged events
- **Choreography**
  - When a single event should trigger multiple events, the initial event saves all required information in a single message
  - Once the initial event completes, the other services asynchronously use the message to complete their respective tasks
- **Log Aggregation**
  - Use a central logging location for all your microservices
- **Polyglot Persistence**
  - Since microservices are interoperable, their only exposure should be public APIs, and the underlying implementation is hidden
  - This means that as long as the service adheres to this capability, it can use different underlying technologies and code to implement it
  - A service with polyglot persistence uses the best data storage technology based on its own data access patterns

# Continuous Integration and Continuous Delivery on AWS

## CI/CD Services on AWS

- [**Cloud9**](https://docs.aws.amazon.com/cloud9/latest/user-guide/welcome.html)
  - A cloud-based IDE to write, run, and debug code from your browser
- [**CodeStar**](https://docs.aws.amazon.com/codestar/latest/userguide/welcome.html)
  - Provides a unified location for managing software deployment activities of defined projects, such as develop, build, and deploy
- [**CodePipeline**](https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html)
  - Fully managed continuous delivery service to automate releases
  - The pipeline's phases and actions in each phase are defined by you
- [**CodeCommit**](https://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html)
  - Fully managed source control service to host Git repositories
- [**CodeBuild**](https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html)
  - Fully managed continuous integration service
  - Compile source code, run tests, and produce software packages
- [**CodeDeploy**](https://docs.aws.amazon.com/codedeploy/latest/userguide/welcome.html)
  - Fully managed deployment service for a variety of compute services
    - Ex. EC2, ECS, Lambda
- [**Amplify Console**](https://docs.aws.amazon.com/amplify/latest/userguide/welcome.html)
  - Continuous delivery service to deploy and host full-stack serverless web applications

## CI/CD Patterns for Different Application Types

- [**Deploy a Single-Page Web Application**](https://docs.aws.amazon.com/whitepapers/latest/modern-application-development-on-aws/deploy-a-single-page-application.html)
- [**Deploy to Containers**](https://docs.aws.amazon.com/whitepapers/latest/modern-application-development-on-aws/deploy-to-containers.html)
- [**Deploy to Containers (Blue/Green Deployment)**](https://docs.aws.amazon.com/whitepapers/latest/modern-application-development-on-aws/deploy-to-containers-bluegreen-deployment.html)
- [**Canary Deployments to AWS Lambda**](https://docs.aws.amazon.com/whitepapers/latest/modern-application-development-on-aws/canary-deployments-to-aws-lambda.html)

# Conclusion

Using modern application development techniques can allow your organization to experiment and innovate rapidly. Development teams that automate repetitive tasks, use managed services when possible, and offload server management for serverless architectures, can spend more time building their core products and improving customer experience. Adopting these best practices can increase the potential for business success.