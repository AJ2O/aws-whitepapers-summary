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

# Characteristics of Microservices Architecture

## Componentization Via Services

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

## Organized Around Business Capabilities

## Products Not Projects

## Smart Endpoints and Dump Pipes

## Decentralized Governance

## Infrastructure Automation

## Design For Failure

## Evolutionary Design

# Conclusion



# References
- [Whitepaper (PDF)](https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/running-containerized-microservices.pdf)
- [Whitepaper (Documentation Format)](https://docs.aws.amazon.com/whitepapers/latest/running-containerized-microservices/welcome.html)
- [martinFowler.com - Microservices](https://martinfowler.com/articles/microservices.html)
- [The Twelve-Factor App](https://12factor.net/)
- [IBM - Containerization](https://www.ibm.com/cloud/learn/containerization)