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
- It uses a **dependency isolation tool** during execution to ensure that no implicit dependencies "leak in" from the system
- Ex. For Python
  - Declaration: [Pip](http://www.pip-installer.org/en/latest/)
  - Isolation: [Virtualenv](http://www.virtualenv.org/en/latest/)

## III. Config


## IV. Backing Services


## V. Build, Release, Run


## VI. Processes


## VII. Port Binding


## VIII. Concurrency


## IX. Disposability


## X. Dev/Prod Parity


## XI. Logs


## XII. Admin Processes


# References
- [12factor.net](https://12factor.net/)
