# **Implementing Microservices on AWS**

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf)

This summary is based off of the August 2019 revision of the **Implementing Microservices on AWS** whitepaper.

In a [**microservices architecture**](https://aws.amazon.com/microservices/), software is composed of small, independent services that communicate over well-defined APIs. It allows for organizations to speed up development cycles, improve scalability of applications, and foster innovation. 

The focus of this whitepaper is to identify the features of a reliable, scalable, and easy-to-manage microservices architecture, and how to build it using AWS container and serverless technologies. Then the whitepaper discusses the cross-service aspects of microservices, such as service discovery, asynchronous communication, and distributed monitoring.

# Simple Microservices Architecture on AWS
A typical microservices architecture splits its functionalities by implementing a specific domain. The following example uses 3 domains: 
- User Interface (UI)
- Microservices
- Data Store

<img src="Diagrams/Microservices.png" alt="Microservices"/>

## User Interface (UI)
- Modern web applications often use JavaScript frameworks to implement single-page applications that communicate using RESTful APIs
  - Static web content can be served using [S3](https://aws.amazon.com/s3/) and [CloudFront](https://aws.amazon.com/cloudfront/)

## Microservices

### Application Programming Interfaces (APIs)
- **APIs** serve as the entry point for application logic, accepting requests from clients, processing the requests, and potentially sending responses back to the calling clients
  - [API Gateway](https://aws.amazon.com/api-gateway/) is a fully managed service that allows developers to publish, monitor, and secure APIs at scale

### Containers
- **Container** technologies like [Docker](https://www.docker.com/) provide benefits such as portability, productivity, and efficiency, but there are complexities when it comes to monitoring and securing the container management infrastructure at scale
  - [Elastic Container Service (ECS)](https://aws.amazon.com/ecs/) and [Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/) are fully managed services that can handle the installation, operation, and scaling of container management infrastructure on the customer's behalf
  - [AWS Fargate](https://aws.amazon.com/fargate/) can be used with ECS or EKS so there is no need to worry about provisioning or scaling the virtual machines that host the containers
- Docker images used with ECS or EKS can be stored in [Elastic Container Registry (ECR)](https://aws.amazon.com/ecr/)

### Serverless Functions
- **Serverless** is defined as an operational model by the following tenets:
  - No infrastructure to provision or manage
  - Automatically scaling by unit of consumption
  - "Pay for value" billing model
  - Built-in availability and fault-tolerance
- [AWS Lambda](https://aws.amazon.com/lambda/) is a serverless compute service where the user uploads code (creating a Lambda function), and Lambda handles everything required to run and scale the function to meet demand
  - Supported programming languages include NodeJS, Python, Go, Java

## Data Store
- Data stores are used to persist data needed by the microservices
- Popular in-memory data stores include Memcached and Redis, both of which can be used fully managed with [ElastiCache](https://aws.amazon.com/elasticache/)
- NoSQL databases are designed to favor the scalability required by microservices, a feature not inherent in relational databases
  - [DynamoDB](https://aws.amazon.com/dynamodb/) is a serverless NoSQL database service with high performance and autoscaling capabilities
  - When the feature [DynamoDB Accelerator (DAX)](https://aws.amazon.com/dynamodb/dax/) is enabled, the database is also provided with caching capabilities

# Reducing Operational Complexity

#

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/microservices-on-aws.pdf)
- [What are Microservices? (AWS)](https://aws.amazon.com/microservices/)