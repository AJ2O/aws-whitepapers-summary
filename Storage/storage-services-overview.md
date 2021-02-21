# **AWS Storage Services Overview**

# Sections
- [**AWS Storage Services Overview**](#aws-storage-services-overview)
- [Sections](#sections)
- [Overview](#overview)
- [Amazon S3](#amazon-s3)
  - [Usage Patterns](#usage-patterns)
  - [Anti-Patterns](#anti-patterns)
  - [Performance](#performance)
  - [Durability and Availability](#durability-and-availability)
  - [Security](#security)
  - [Cost Model](#cost-model)
- [Amazon Glacier](#amazon-glacier)
- [Amazon EFS](#amazon-efs)
- [Amazon EBS](#amazon-ebs)
- [Amazon EC2 Instance Storage](#amazon-ec2-instance-storage)
- [AWS Snowball](#aws-snowball)
- [Amazon CloudFront](#amazon-cloudfront)
- [Conclusion](#conclusion)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf)

This summary is based off of the December 2016 revision of the **AWS Storage Services Overview** whitepaper. It covers various AWS storage solutions and their specifics, including usage patterns, performance, security, and other factors.

# Amazon S3
[Amazon S3](https://aws.amazon.com/s3/) is a scalable and highly durable key/object storage service. It can be used to store and retrieve any amount of data at any time through a simple web interface.

## Usage Patterns
There are four common uses for S3:
- **1. Distribute static web content and media**
  - Each object has a unique HTTP URL, which can be used to access the content
  - Ex. Images, Videos
- **2. Host static websites**
  - Ex. HTML files, static web content, client-side JavaScript
- **3. Store data for computation and large-scale analytics**
  - S3's horizontal scalability allows simultaneous access from multiple compute nodes with high performance
  - Ex. Financial transaction analysis, clickstream analytics, media transcoding
- **4. Backup and archiving of critical data**
  - S3 is durable, scalable, and be secured with encryption

## Anti-Patterns
The following are storage needs for which other AWS services are a better choice than S3:
- **File system**
  - S3 uses a flat namespace and isn't meant to serve as a standalone, [POSIX-compliant](https://en.wikipedia.org/wiki/POSIX) file system
  - Use [EFS](http://aws.amazon.com/efs/) instead
- **Structured data with query**
  - S3 doesn't offer query capabilities to retrieve specific objects
  - Use database services such as [RDS](https://aws.amazon.com/rds/) or [DynamoDB](https://aws.amazon.com/dynamodb/) instead
- **Dynamic website hosting**
  - Dynamic websites depend on database interaction or server-side scripting
  - Create dynamic websites on [EC2](https://aws.amazon.com/ec2/) or [EFS](http://aws.amazon.com/efs/) instead

## Performance
- S3 is designed so that server-side latencies are insignificant relative to Internet latencies
- It scales storage and requests to support an extremely large amount of web-scale applications
- To improve upload performance of large objects (above 100 MB), S3 offers multi-part uploads for improved throughput and quick recovery from network issues
- [S3 Transfer Acceleration (S3TA)](https://aws.amazon.com/s3/transfer-acceleration/) can be enabled to greatly improve upload speeds (up to 500% faster) over long distances

## Durability and Availability
- S3 buckets (in Standard or Standard-IA class) automatically replicate data across multiple devices and facilities in the bucket's geographical region
  - 99.999999999% ("11 nines") durability over 1 year
  - 99.99% availability over 1 year
- Cross-region replication can be enabled on a bucket to automatically copy objects to a different AWS region when items are uploaded to it

## Security
- **Access**
  - Fine-grained access by AWS accounts and users can be granted or denied at the object and/or bucket level using [access policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-policy-alternatives-guidelines.html)
- **Encryption**
  - S3 supports encryption at rest, either using AWS managed keys, or custom keys
  - S3 data can be protected in transit using SSL or client-side encryption
- **Monitoring**
  - Requests to a bucket can be tracked by enabling [access logging](http://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html)
  - Each record contains detailed request specifics such as the requester, request time, request action, response status, etc.
  - Helpful for security audits, learning the customer base, and understanding the S3 bill

## Cost Model
- Pay only for the storage actually used
- **3 pricing components:**
  - Storage (per GB/month)
  - Data transfer in or out (per GB/month)
  - Requests (per thousand requests/month)

# Amazon Glacier


# Amazon EFS


# Amazon EBS


# Amazon EC2 Instance Storage


# AWS Snowball


# Amazon CloudFront


# Conclusion

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf)