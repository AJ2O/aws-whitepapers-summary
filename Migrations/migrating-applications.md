# **Migrating Applications Running Relational Databases to AWS**

# Sections
- [**Migrating Applications Running Relational Databases to AWS**](#migrating-applications-running-relational-databases-to-aws)
- [Sections](#sections)
- [Overview](#overview)
- [Migrating Data-Centric Applications to AWS](#migrating-data-centric-applications-to-aws)
  - [Migration Steps](#migration-steps)
  - [Migration Tools](#migration-tools)
    - [AWS Schema Conversion Tool (AWS SCT)](#aws-schema-conversion-tool-aws-sct)
    - [AWS Database Migration Service (AWS DMS)](#aws-database-migration-service-aws-dms)
  - [Step 1: Migration Assessment](#step-1-migration-assessment)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf)

This summary is based off of the February 2017 revision of the **Migrating Applications Running Relational Databases to AWS** whitepaper. It provides an overview of the steps to migrate a relational database to the cloud, and the tools that can greatly aid in this effort: the [AWS Schema Conversion Tool (SCT)](https://aws.amazon.com/dms/schema-conversion-tool/) and [AWS Data Migration Service (DMT)](https://aws.amazon.com/dms).

# Migrating Data-Centric Applications to AWS

## Migration Steps
The steps for a database migration, regardless of database engine, are listed below in order. These will be explained in detail in their own sections. Associated with each step is the average percentage of time spent in each step for a typical application:

<html>
<table align="center">
    <tr>
        <th>Step</th>
        <th>Percentage of Overall Effort</th>
    </tr>
    <tr>
        <th>1. Migration Assessment</th>
        <td>2%</td>
    </tr>
    <tr>
        <th>2. Schema Conversion</th>
        <td>30%</td>
    </tr>
    <tr>
        <th>3. Embedded SQL and Application Code Conversion</th>
        <td>15%</td>
    </tr>
    <tr>
        <th>4. Data Migration</th>
        <td>5%</td>
    </tr>
    <tr>
        <th>5. Testing</th>
        <td>45%</td>
    </tr>
    <tr>
        <th>6. Data Replication</th>
        <td>3%</td>
    </tr>
    <tr>
        <th>7. Go Live</th>
        <td>5%</td>
    </tr>
</table>
</html>

## Migration Tools
### AWS Schema Conversion Tool (AWS SCT)
The SCT is a desktop tool that automates conversion of database objects from different database migration systems to different [Amazon RDS](https://aws.amazon.com/rds/) database targets. This tool is invaluable during the Migration Assessment, Schema Conversion, and Application Code Conversion steps.

### AWS Database Migration Service (AWS DMS)
DMS is a service data migration to and from AWS database targets. DMS can be used for a variety of replication tasks, such as:
- continuous replication to offload reads from a primary production server
- continuous replication for high availability
- database consolidation
- **temporary replication for data migrations**

This document will focus on the replication needed for data migrations, reducing the time and effort during the Data Migration and Data Replication Setp steps.

## Step 1: Migration Assessment

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf)
