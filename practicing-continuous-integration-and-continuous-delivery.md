# **Practicing Continuous Integration and Continuous Delivery on AWS**

# Overview
- [Source Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/practicing-continuous-integration-continuous-delivery.pdf) - 33 Pages
- [Documentation Format](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/welcome.html)
- [What is Continuous Integration?](https://aws.amazon.com/devops/continuous-integration/)
- [What is Continuous Delivery?](https://aws.amazon.com/devops/continuous-delivery/)

This summary is based off of the June 2017 revision of the Practicing Continuous Integration and Continuous Delivery on AWS whitepaper.

**Continuous Integration** and **Continuous Delivery**, referred to as **CI/CD** for short, are two principles under the umbrella of [DevOps](http://aws.amazon.com/devops/what-is-devops/). DevOps principles aim for increasing the speed at which organizations develop, improve, and deploy their products. These principles can give companies the edge in serving their customers when compared to using traditional software development practices.

# What Is Continuous Integration & Continuous Delivery/Deployment?

## Continuous Integration
**Continuous Integration (CI)** is a software development practice where developers regularly merge their code changes into a central repository, with each merge starting automated builds and running tests against the build. Key characteristics of CI:
- **Frequent Code Changes**
  - Code changes occur multiple times per day, triggering builds with incremental improvements over previous iterations
- **Small Code Changes**
  - Small code changes have a smaller chance of generating errors, and are easier to fix if an error is discovered
- **Automated Builds**
  - Each code change automatically triggers a build on a build server, eliminating the need for a person to manually run build steps
- **Automated Tests**
  - Once a build is generated, automated tests run against it. If tests fail, the build server rejects the commit, and may send a notification to the developer who caused the error
  - Immediate feedback allows the developer to fix the issue as soon as possible

## Continuous Delivery and Deployment
**Continuous Delivery (CD)** is an expansion of CI, where the automated build can be deployed to one or more environments after the build has completed. CD doesn't guarantee the specific build is deployed yet, but it can be, whether automatically or with manual approval.

**Continuous Deployment** is the full end-to-end automation of CI/CD to a production environment, without any manual approval step. This differs from CD, because CD may contain manual approval steps before committing to a production deployment.

## Benefits of Continuous Delivery
- **Automate the Software Release Process**
  - Provides a consistent, rapid, and efficient method for the team to deploy code changes to production
- **Improve Developer Productivity**
  - Frees developers from manual tasks, and they can focus on delivering new product features
- **Improve Code Quality**
  - Discover and address bugs early in the delivery process before they become large problems later
  - Errors caught early are the easiest to fix
- **Deliver Updates Faster**
  - Features and updates are delivered quickly and frequently
  - Allows the team to respond faster to customer needs, market changes, new security vulnerabilities, etc.

# Implementing Continuous Integration and Continuous Delivery

## A Pathway to Continuous Integration/Continuous Delivery
CI/CD can be represented as a pipeline with ordered stages. The number and variation of the stages can be adapted to fit your product or application, but there are generally four distinct stages:
1. **Source**
    - Developers commit changes to a central code repository
2. **Build**
    - Changes trigger an automated build
3. **Staging**
    - The build goes through automated tests
    - Tests can include simple unit tests, or deploying to a test/staging environment for more rigorous testing
4. **Production**
    - The code is deployed to public servers

Each stage has a dependency on the last, and the code quality is assumed to be higher as it passes through the stages. As your organization's CI/CD pipeline matures, there are some other improvements that can be added to it:

- Different staging environments for specific performance, security, and UI tests
- Unit tests of infrastructure and configuration code along with application code
- Integration with other processes such as code review, issue tracking, and event notification
- Additional steps for auditing and business approval


## Teams
AWS recommends three developer teams for implementing a CI/CD environment: application, infrastructure, and tools. Teams should be no larger than 10-12 people, as this limit follows the [communication rule](http://www.businessinsider.com/jeff-bezos-two-pizza-rule-for-productive-meetings-2013-10) that meaningful conversations hit limits as group sizes increase.

- **Application Team**
  - Develops the application
  - Should have programming skills in the application language
  - Should have an understanding of the system configuration
  - Should **not** spend time on non-core application tasks
- **Infrastructure Team**
  - Writes and configures the infrastructure needed to run the application
  - Works closely with the application team
  - Should have skills in infrastructure provisioning methods
    - Ex. CloudFormation, Terraform
  - Should have skills and experience with configuration automation tools
    - Ex. Chef, Puppet, Ansible
- **Tools Team**
    - Builds and manages the CI/CD pipeline, including the infrastructure and tools that make up the pipeline
      - Ex. Source control repositories, build environments, testing frameworks, artifact repositories
    - AWS Toolset includes:
      - CodeCommit, CodePipeline, CodeStar
    - Popular Third-Party Tools include:
      - GitHub, Jenkins, Artifactory

## Testing Stages in Continuous Integration and Continuous Delivery 
Referring back to the 4 basic CI/CD stages, there are specific test types that can be run in each stage.

### Source
This is the beginning stage where code is simply stored, so there's no tests to be run during this stage.

### Build
The tests in this stage do not need a full environment to run, so they are the cheapest and fastest test types. Bugs caught in this phase are the quickest and cheapest to fix.

- **Unit Testing**
  - Tests a specific section of code to ensure that it does what it's expected to do
  - Should be a majority [(~70%)](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/testing-stages-in-continuous-integration-and-continuous-delivery.html) of your testing strategy, and have near code-complete coverage
- **Static Code Analysis**
  - Detect coding errors, security holes, or ensure conformance to coding guidelines
  - Can be performed without actually executing the application after the build

### Staging
The tests in this stage require more detailed environments, and some even need to mirror the eventual production environment. As such, they are more costly for infrastructure requirements and are slower to run than those in the Build stage. There are 3 sub-categories separated by their associated cost, which accounts for infrastructure costs and test speed.

These 3 test types are the cheapest in this stage:
- **Component Testing**
  - Tests messaging between various application components and their outcomes
- **Integration Testing**
  - Verifies the interfaces between components against software design
- **System Testing**
  - Tests the system end-to-end and verifies the software satisfies the business requirement

These 2 test types are the next-highest cost in this stage:
- **Compliance Testing**
  - Checks whether the code change meets the defined standards of a particular non-functional requirement (ex. regulations)
- **Performance Testing**
  - Determines the responsiveness and stability of a system as it performs under a particular workload
  - May include load tests, stress tests, and spike tests

This test type is the most expensive in this stage:
- **User Acceptance Testing / UI Testing**
  - Validates the end-to-end business flow
  - Is manually executed by an end user in a staging environment

### Production
- **Canary Test**
  - Deploy the new application version to a small subset of production servers and monitor it according to your pre-defined specifications
  - After the canary satisfies your requirements, deploy it to the entire production environment

# Deployment Methods

- **All at Once / In-Place Deployment**
  - Deploys the new version to all servers at once
- **Rolling Deployment**
  - Deploys the new version to sections of servers one at a time
    - If a deployment fails, only one section of the servers will be down
- **Immutable Deployment**
  - Deploys the new version to a new fleet of servers, and deletes the old servers
- **Blue/Green Deployment**
  - Keeps two isolated environments, a "Blue" environment and a "Green" environment
  - The "Green" environment has the new version deployed to it
  - The "Blue" environment is the old environment in-case a rollback is needed

# Conclusion
When CI/CD is used, code changes are integrated, tested, and deployed with reliability and speed every time. Aside from velocity, this allows for software updates to be delivered with confidence since each change goes through a standardized process. It also removes repetitive and manual steps, so developers can focus on improving their applications and products.

# References
- [Whitepaper (PDF)](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/practicing-continuous-integration-continuous-delivery.pdf)
- [Whitepaper (Documentation Format)](https://docs.aws.amazon.com/whitepapers/latest/practicing-continuous-integration-continuous-delivery/welcome.html)
- [What is DevOps? (AWS)](https://aws.amazon.com/devops/what-is-devops/)
- [What is Continuous Integration? (AWS)](https://aws.amazon.com/devops/continuous-integration/)
- [What is Continuous Delivery? (AWS)](https://aws.amazon.com/devops/continuous-delivery/)