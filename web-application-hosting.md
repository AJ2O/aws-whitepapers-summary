# **Web Application Hosting in the AWS Cloud**

# Overview
- [Source Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/web-application-hosting-best-practices.pdf) - 21 Pages
- [Documentation Format](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/welcome.html)

This summary is based off of the September 2019 revision of the Web Application Hosting in the AWS Cloud: Best Practices whitepaper. It describes the best practices and tools available to allow scalability, availability, security, high performance of web applications in the cloud.

# Web Application Hosting in the Cloud Using AWS

## How AWS Can Solve Common Web Application Hosting Issues
- **Oversized Fleets Needed to Handle Peaks**
  - In the traditional model, you have to provision enough servers to handle peak capacity for your application, but this means that this extra space is wasted outside of peak hours
  - AWS can provide on-demand auto-scaling to increase or decrease your amount of servers to match your traffic patterns, saving costs during non-peak hours
- **Handling Unexpected Traffic Peaks**
  - In the traditional model, you have to provision servers ahead of time, meaning you can't react quickly to random traffic spikes
  - AWS on-demand auto-scaling can also handle unexpected load spikes, and once again scale down once traffic subsides
- **Test, Load, Beta, and Preproduction Environments**
  - You can provision testing fleets of servers as you need them, and remove them once they're no longer required, saving cost

## An AWS Cloud Architecture for Web Hosting
A common web hosting architecture consists of 3 layers:
- Presentation
  - Ex. Exterior Firewall, Web Load Balancer, Web Servers
- Application
  - Ex. Application Load Balancer, Application Servers
- Persistence
  - Ex. Databases, Tape Backups

A diagram of a traditional model is below:
![Traditional_Web_App](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/images/image2.png)

The AWS implementation of this model below: 
![AWS_Web_App](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/images/image4.png)

## Key Components of an AWS Web Hosting Architecture

- **Network Management**
- **Content Delivery**
  - Edge Caching with [CloudFront](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/content-delivery.html)
- **Managing Public DNS**
  - DNS routing with [Route53](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/managing-public-dns.html)
- **Host Security**
  - [Security Groups](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/host-security.html) can be considered as network firewalls at the instance level
- **Load Balancing Across Clusters**
- **Finding Other Hosts and Services**
  - A persistent database can be used to as the common repository for service and host discovery
- **Caching within the Web Application**
  - Can reduce the load on services
  - Can improve performance and stability on the database tier
  - [ElastiCache](https://aws.amazon.com/elasticache/) is a fully managed, in-memory caching service
- **Database Configuration, Backup, and Failover**
<table>
  <tr>
    <th>Management</th>
    <th>Relational Database Solution</th>
    <th>NoSQL Solutions</th>
  </tr>
  <tr>
    <td>Managed Database Service</td>
    <td>RDS - Aurora, MariaDB, MySQL, Oracle, PostgreSQL, SQL Server</td>
    <td>DynamoDB</td>
  </tr>
  <tr>
    <td>Self-Managed</td>
    <td>Hosting a relational DBMS on an EC2 instance</td>
    <td>Hosting a NoSQL solution on an EC2 instance</td>
  </tr>
</table>

- **Storage and Backup of Data and Assets**
  - [S3](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/storage-and-backup-of-data-and-assets.html) for somewhat static objects like images and videos
  - [EBS](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/storage-and-backup-of-data-and-assets.html) for host storage, backups, and snapshots
- **Automatically Scaling The Fleet**
- **Additional Security Features**
  - [AWS Shield](https://aws.amazon.com/shield/) is a managed DDoS protection service
  - [AWS WAF](https://aws.amazon.com/waf/) is a customizable firewall service that can also protect your applications from attacks such as cross-site scripting or SQL-injection
- **Failover with AWS**
  - Take advantage of [availability zones](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/failover-with-aws.html) and deploy your web servers across them to enable persistence of your application
    - Ex. [RDS](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/failover-with-aws.html) can utilize multi-AZ deployments, so that if your primary database goes down, the secondary database (with replicated data) automatically becomes the new primary database, with little to no downtime

# Key Considerations When Using AWS for Web Hosting


# Conclusion


# References
- [Whitepaper (PDF)](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/web-application-hosting-best-practices.pdf)
- [Whitepaper (Documentation Format)](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/welcome.html)