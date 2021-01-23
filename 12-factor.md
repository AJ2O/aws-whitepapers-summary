# **The Twelve-Factor App**

# Overview

Modern software is usually delivered as a service in the form of *web apps* or *software-as-a-service (SaaS)*. The **twelve-factor app** is a methodology for building SaaS apps and can be applied to any programming language. Some features achieved by this methodology include:
- **app isolation** from operating systems and underlying execution environments
- **minimized differences** between development and production environments
- **scaling** without major changes to architecture, tooling, or development practices
- **declarative formats** to automate setup of apps

# The Twelve Factors

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
  - Resource handles to the database, Memcached, and other [backing services](https://12factor.net/backing-services)
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
  - The service is only accessed via URL or other credentials stored in the [config](https://12factor.net/config)
  - Allows for easily swapping services between deploys by modifying the config

## V. Build, Release, Run
**Strictly separate build and run stages**

A codebase is transformed into a deploy through 3 separate stages:
1. **Build**
  - Converts a code repo into an executable bundle known as a *build*
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
- The port-binding approach means that one app can become the backing service for another consuming app
  - The URL to the backing app can be provided to the consuming app via its config
- Ex. Webserver
  - Export HTTP to port 8000
  - Visiting `http://<host-ip>:5000` accesses the web service

## VIII. Concurrency


## IX. Disposability


## X. Dev/Prod Parity


## XI. Logs


## XII. Admin Processes


# References
- [12factor.net](https://12factor.net/)
