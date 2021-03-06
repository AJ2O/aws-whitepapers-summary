# **Running Containerized Microservices on AWS**

# Overview
- [Source Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/running-containerized-microservices.pdf) - 24 Pages
- [Documentation Format](https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/welcome.html)

This summary is based off of the November 2017 revision of the Running Containerized Microservices on AWS whitepaper.

# Introduction

**Microservices architecture** is an approach to software development in which a single application is composed of smaller "microservices", with each running independently within its own process, and they communicate with each other using well-defined APIs. This approach also organizational, as each service has an associated development team. 

**Containers** provide isolation and software packaging, so each service would be allocated its own container, and the containers work together to provide the application's overall functionality.

Microservices are often compared to the traditional **monolithic architecture**. In a monolithic application, all of its components run under a single process. While it is a simple way to build software, it has its fair share of issues. For example, a small code change to part of the application requires the whole application to be rebuilt and deployed. It's also more difficult to keep code changes in one part of the application from inadvertently affecting another part. Finally, scaling (such as in response to demand) requires scaling the whole monolith, even if some sections of application don't require scaling.

Microservices architecture provides many benefits, such as easier scalability of application components, fault isolation, and quicker development times. There's no concrete definition of the microservices architecture style, but this whitepaper will describe the general characteristics that fit this label.

### Characteristics of Microservices Architecture
- **Componentization via Services**
- **Organized Around Business Capabilities**
- **Products not Projects**
- **Smart Endpoints and Dump Pipes**
- **Decentralized Governance**
- **Decentralized Data Management**
- **Infrastructure Automation**
- **Design for Failure**
- **Evolutionary Design**

# The Twelve-Factor App
To achieve these microservice characteristics, the [Twelve-Factor App](https://12factor.net/) pattern methodology can help to provide an outline. These factors are a set of best practices for developing modern applications to be suitable for deployment on modern cloud platforms:

1. **Codebase** 
   - One codebase tracked in revision control, many deploys
2. **Dependencies** 
   - Explicitly declare and isolate dependencies
3. **Config** 
   - Store configuration in environment variables
4. **Backing services** 
   - The code makes no distinction between local and third party services
5. **Build, release, run** 
   - Strictly separate build and run stages
6. **Processes** 
   - Execute the app as one or more stateless processes
7. **Port Binding** 
   - Export services via port binding
8. **Concurrency** 
   - Scale out via the process model
9.  **Disposability** 
    - Maximize robustness with fast startup and graceful shutdown
10. **Dev/prod parity** 
    - Keep development, staging, and production as similar as possible
11. **Logs** 
    - The application never concerns itself with routing or storage of its log stream
12. **Admin Processes** 
    - Run administration & management tasks as one-off processes

# Componentization Via Services

To help explain componentization, let's use a car for an analogy. A car is composed of multiple components: the engine, battery, and wheels just to name a few. If we blow out a tire, it can easily be replaced with another one. If the battery ages and doesn't work properly, we can simply replace it for a newer, better-performing battery. The theme is that none of these issues require replacing the entire vehicle, but we can swap in and out parts as we choose to.

**Componentization** of  an application works the same way. Each microservice should be its own independent, scalable, and replaceable component, without affecting or interrupting other components in the application. 

**Containerization** can be used to achieve componentization, as it is an operating-system-level virtualization method. Virtual machines allow for multiple applications to run on one server, but containers take this one step further and allow for multiple applications to run on one operating system, with each running as its own isolated process. Each microservice's total functionality is built into its container image, so we can easily swap and upgrade the service by modifying its underlying image.

### Relevant Twelve-Factors
- **Dependencies**
   - Dependencies are self-contained within the container and not shared with other services
- **Concurrency**
   - Each container (or groups of containers at a time) can be auto-scaled independently in a memory- and CPU-efficient manner
- **Disposability**
   - Containers are easily be discarded when they stop running, and are re-built from an image repository

# Organized Around Business Capabilities

Services should be organized around **business capabilities**. This means that for each microservice (or small group of microservices), its associated team must own all aspects of it, from development all the way to the customer. This may include development, deployment, user-interface, databases, production support and other aspects.

When architecture is organized into defined business capabilities, it allows for dependencies between components to be loosely coupled, and for teams to work independently without being reliant on, or affecting other components.

### Relevant Twelve-Factors
- **Codebase**
  - Each microservice has its own codebase in a separate repository
- **Build, release, run**
  - Each microservice has its own deployment pipeline and frequency
- **Processes**
  - Each microservice does one thing, and does that one thing very well
- **Admin processes**
  - Each microservice has its own administration tasks so that it functions as designed

# Products Not Projects

Treat software components as **products** that can be progressively improved and are constantly evolving. This is in contrast to handling components as **projects** that are developed by an engineering team and handed off to an operations team responsible for running it.

### Essentials to Adopting a "Product Mindset"
- **Automated Provisioning**
  - Operations should be automated rather than manual, increasing velocity
- **Self-service**
  - Engineers should be able to configure and provision their own dependencies
- **Continuous Integration**
  - Engineers should check in code frequently so that incremental improvements are available for review and testing as quickly as possible
  - [Read More on Continuous Integration](https://aws.amazon.com/devops/continuous-integration/)
- **Continuous Build and Delivery**
  - Automate the process of building code and delivering it to production
  - [Read More on Continuous Delivery](https://aws.amazon.com/devops/continuous-delivery/)

### Relevant Twelve-Factors
- **Build, release, run**
  - Engineers adopt a [DevOps](https://aws.amazon.com/devops/what-is-devops/) culture that allows them to optimize all three stages
- **Config**
  - Engineers build better configuration management for software because of their involvement with how that software is used by the customer
- **Dev/prod parity**
  - Software treated as a product can be iteratively developed in smaller pieces that take less time to complete and deploy, allowing development and production to be closer in parity

# Smart Endpoints and Dumb Pipes

In a microservices architecture, an environment can have its components distributed across a group of servers, meaning that services may not even be located on the same server. For them to communicate, there are two primary methods:
- **Request/Response**
  - One service explicitly invokes another service by making a request to either store data in it or retrieve data from it.
- **Publish/Subscribe**
  - One service "A" (or many) subscribes to another service "B" to watch for events. When service "B" publishes events, any subscribed services will be invoked implicitly.

Building **smart endpoints** means concentrating the production and consumption logic of requests in each service. **Dumb pipes** refer to refraining from coupling services to specific and complicated messaging frameworks, which keeps the transferred requests simple, or "dumb".

### Relevant Twelve-Factors
- **Port Binding**
  - Services bind to a port to watch for incoming requests or to send requests to ports of other services
- **Backing services**
  - Dumb pipes allow for a background service to be attached to another service in the same way you can attach a database
- **Concurrency**
  - Several observer service can respond and work in parallel to a single event produced by another service

# Decentralized Governance

With **decentralized governance**, each team can use its expertise to select the best tools to develop and manage their associated service. 

Different services solve different problems, so teams can use the best tools that address their specific problem. This means that two teams can use two different types of databases, or even two different coding languages for their service. This feature doesn't necessitate using different solutions for different teams, but it at least presents the option.

### Relevant Twelve-Factors
- **Dependencies**
  - Teams can choose their own dependencies, so dependencies should be isolated
- **Build, release, run**
  - Teams can use different build processes, but releasing and running the code should still be seamless
- **Backing services**
  - Services can be refactored or developed in different ways, as long as they obey an external contract for communication with other services

# Decentralized Data Management

**Decentralized data management** encompasses that each service gets its own data storage and doesn't directly share its data with any other service. This also means there is no shared, external database for services to access. 

Data should only be communicated through smart endpoints and dumb pipes. This allows teams to choose the best data storage solution for their service, whether it be an object store, key-value store, file store, block store, or a relational database.

### Relevant Twelve-Factors
- **Disposability**
  - Services are robust and not dependent on externalities
- **Backing services**
  - A backing service is any service that the app consumes over the network such as data stores and messaging systems
  - The app should make no distinction between a local and an external service
- **Admin processes**
  - Admin processes should be run in a similar manner, irrespective of environments

# Infrastructure Automation

An automated infrastructure provides repeatability for quickly setting up environments. Using **infrastructure as code**, it can be checked into source control to allow for easily deploying different types of infrastructure, and easily rolling back if an error occurs. This reduces the risk of change and as a result, promotes innovation and experiments.

### Relevant Twelve-Factors
- **Codebase**
  - Infrastructure can be described as code, so treat all code similarly and keep it in the service repository
- **Config**
  - The environment should hold and share its own specificities
- **Build, release, run**
  - One environment for each purpose (ex. development, integration, performance testing, production)
- **Dev/prod parity**
  - Keep infrastructure code for development, staging, and production similar

# Design For Failure

As a consequence of services being components, applications need to be designed so that they can tolerate the failure of services. Connections between services need to be monitored and managed. Latency and timeouts should be assumed and gracefully handled. Since services can fail at any time, so it's important to detect failures quickly, and if possible, automatically restore the service. 

Designing for failure also means regularly testing the design and monitoring how services cope with the deteriorating conditions. There are various ways of simulating deteriorating conditions, as explained in the tech blog: [The Netflix Simian Army](https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116).

### Relevant Twelve-Factors
- **Disposability**
  - Produce smaller container images and strive for processes that can start and stop in seconds
- **Logs**
  - If part of a system fails, troubleshooting is necessary
  - Ensure that logs and metrics exist to aid in troubleshooting

# Evolutionary Design

Having a detailed design phase at the very beginning of a project is impractical, and services have to evolve through various iterations of the software. This is done through learning from real-world usage of the service.

A service team should build the minimum viable set of features needed to roll out the service to users. New features are tested by rolling them out in a [canary release](https://martinfowler.com/bliki/CanaryRelease.html), and once customer feedback comes in, the team evolves the service in response. At a later stage, the team can decide to refactor the service after they feel confident that they have enough feedback.

### Relevant Twelve-Factors
- **Codebase**
  - Helps evolve features faster since new feedback can be quickly incorporated
- **Dependencies**
  - Enables quick iterations of the design since features are tightly coupled with externalities
- **Config**
  - Configuration is stored in the environment, outside of the code
  - This allows
- **Build, release, run**
  - Use various deployment techniques to help roll out new features
  - Each release has a specific ID and can be used to gain design efficiency and user feedback

# References
- [Whitepaper (PDF)](https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/running-containerized-microservices.pdf)
- [Whitepaper (Documentation Format)](https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/welcome.html)
- [martinFowler.com - Microservices](https://martinfowler.com/articles/microservices.html)
- [The Twelve-Factor App](https://12factor.net/)
- [IBM - Containerization](https://www.ibm.com/cloud/learn/containerization)
- [Netflix Tech Blog - The Netflix Simian Army](https://netflixtechblog.com/the-netflix-simian-army-16e57fbab116)