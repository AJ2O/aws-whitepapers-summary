# **The Twelve-Factor App**

# Overview

Modern software is usually delivered as a service in the form of *web apps* or *software-as-a-service (SaaS)*. The **twelve-factor app** is a methodology for building SaaS apps and can be applied to any programming language. Some features achieved by this methodology include:
- **app isolation** from operating systems and underlying execution environments
- **minimized differences** between development and production environments
- **scaling** without major changes to architecture, tooling, or development practices
- **declarative formats** to automate setup of apps

# The Twelve Factors

- **[I. Codebase](#i-codebase)**
- **[II. Dependencies](#ii-dependencies)**
- **[III. Config](#iii-config)**
- **[IV. Backing Services](#iv-backing-services)**
- **[V. Build, Release, Run](#v-build-release-run)**
- **[VI. Processes](#vi-processes)**
- **[VII. Port Binding](#vii-port-binding)**
- **[VIII. Concurrency](#viii-concurrency)**
- **[IX. Disposability](#ix-disposability)**
- **[X. Dev/Prod Parity](#x-devprod-parity)**
- **[XI. Logs](#xi-logs)**
- **[XII. Admin Processes](#xii-admin-processes)**


## I. Codebase
**One codebase tracked in revision control, many deploys**

- A **codebase** is any single repo, or a set of repos that share a root commit (such as [Git](https://git-scm.com/))
  - There is always a **one-to-one** correlation between a codebase and an app
- A **deploy** is a running instance of the app, such as a production site, or multiple staging sites
  - Additionally, every dev has their own local environment, each of which also qualifies as a deploy
- The codebase is the same for all deploys, but each deploy may contain a different version


## II. Dependencies
**Explicitly declare and isolate dependencies**

- A twelve-factor app **never relies on implicit existence of system-wide packages**
- It declares all dependencies via a **dependency declaration manifest**
- It uses a **dependency isolation tool** during execution to ensure that no implicit dependencies "leak in" from the surrounding system
- Ex. For Python
  - Declaration: [Pip](http://www.pip-installer.org/en/latest/)
  - Isolation: [Virtualenv](http://www.virtualenv.org/en/latest/)

## III. Config
**Store config in the environment**

- An app's **config** is anything likely to vary between deploys, such as:
  - Resource handles to the database, caching systems, and other [backing services](#iv-backing-services)
  - Credentials to external services
  - Canonical hostname for the deploy
- Config should be stored in **environmental variables** (shortened to *env vars*)
  - Env vars are easy to change between deploys, and are a standard across programming languages

## IV. Backing Services
**Treat backing services as attached resources**

- A **backing service** is any service the app consumes over the network as part of its normal operation
  - Ex. databases, messaging/queueing systems, mail services, caching systems
- Each distinct backing service is a **distinct resource**
- A twelve-factor app **makes no distinction between local and third party services**
  - The service is only accessed via URL or other credentials stored in the [config](#iii-config)
  - Allows for easily swapping services between deploys by modifying the config

## V. Build, Release, Run
**Strictly separate build and run stages**

A codebase is transformed into a deploy through 3 separate stages:
1. **Build**
  - Converts a [code repository](#i-codebase) into an executable bundle known as a *build*
2. **Release**
  - Takes the build produced by the build stage and combines it with the deploy's *config*, resulting in a *release*, ready for immediate execution
  - Every release should have its own unique ID
3. **Run**
  - Runs the app in the execution environment, by launching some set of the app's processes against a selected release

## VI. Processes
**Execute the app as one or more stateless processes**

- Twelve-factor processes are **stateless** and **share-nothing**
  - Any data that needs to persist must be stored in a stateful backing service, such as a database
- The app should never assume that anything cached in memory or on disk will be available on a future request or job
  - A future request should be able to be served by a different process with no issue, hence being stateless

## VII. Port Binding
**Export services via port binding**

- The app exports services by binding them to a port, and listen to requests coming in on that port
- The port-binding approach means that one app can become the [backing service](#iv-backing-services) for another consuming app
  - The URL to the backing app can be provided to the consuming app via its [config](#iii-config)
- Ex. Webserver
  - Export HTTP to port 8000
  - Visiting `http://<host-ip>:5000` accesses the web service

## VIII. Concurrency
**Scale out via the process model**

- To handle diverse workloads, each type of work should be assigned to a **process type**
  - Example: HTTP requests would be handled by a *web process*
  - Example: Long-running background tasks would be handled by a *worker process*
- To scale out the app, just add more running processes of the designated process types
- This allows for scaling individual components of the app as needed
  - Example: If the app is receiving very high HTTP traffic, only the web process needs to be scaled out, rather than the entire app

## IX. Disposability
**Maximize robustness with fast startup and graceful shutdown**

- Processes should be **disposable**, meaning they can be started or stopped any time
  - Facilitates elastic scaling and rapid deployment of [code](#i-codebase) or [config](#iii-config) changes
- Processes should strive to **minimize startup time**
  - From the launch command to being ready to receive requests, this should take a few seconds
- Processes should **shut down gracefully** when they receive a [SIGTERM](https://en.wikipedia.org/wiki/Signal_(IPC)#SIGTERM) signal
  - This may include to stop listening for requests, allow current requests to complete, then exiting
- Processes should be **robust against sudden death**, such as the case of hardware failure
  - Example: use a queueing backend that returns jobs to the queue when clients disconnect or time out, such as [Beanstalkd](https://beanstalkd.github.io/) or [Amazon Simple Queue Service (SQS)](https://aws.amazon.com/sqs/)

## X. Dev/Prod Parity


## XI. Logs


## XII. Admin Processes


# References
- [12factor.net](https://12factor.net/)
