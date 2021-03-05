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
  - [Step 2: Schema Conversion](#step-2-schema-conversion)
  - [Step 3: Embedded SQL and Application Code Conversion](#step-3-embedded-sql-and-application-code-conversion)
  - [Step 4: Data Migration](#step-4-data-migration)
  - [Step 5: Testing Converted Code](#step-5-testing-converted-code)
  - [Step 6: Data Replication](#step-6-data-replication)
  - [Step 7: Go Live (Deployment to AWS)](#step-7-go-live-deployment-to-aws)
- [References](#references)

# Overview
- [Source](https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf)

This summary is based off of the February 2017 revision of the **Migrating Applications Running Relational Databases to AWS** whitepaper. It provides an overview of the steps to migrate a relational database to the cloud, and the tools that can greatly aid in this effort: the [AWS Schema Conversion Tool (SCT)](https://aws.amazon.com/dms/schema-conversion-tool/) and [AWS Data Migration Service (DMT)](https://aws.amazon.com/dms).

# Migrating Data-Centric Applications to AWS

## Migration Steps
The steps for a database migration, regardless of database engine, are listed below in order. Each of the steps will be explained further in their own sections. Associated with each step is the average percentage of time spent in each step for a typical application:

<html>
<table>
    <tr>
        <th align="left">Migration Step</th>
        <th>Percentage of Overall Effort</th>
    </tr>
    <tr>
        <th align="left">1. Migration Assessment</th>
        <td>2%</td>
    </tr>
    <tr>
        <th align="left">2. Schema Conversion</th>
        <td>30%</td>
    </tr>
    <tr>
        <th align="left">3. Embedded SQL & Application Code Conversion</th>
        <td>15%</td>
    </tr>
    <tr>
        <th align="left">4. Data Migration</th>
        <td>5%</td>
    </tr>
    <tr>
        <th align="left">5. Testing</th>
        <td>45%</td>
    </tr>
    <tr>
        <th align="left">6. Data Replication</th>
        <td>3%</td>
    </tr>
    <tr>
        <th align="left">7. Go Live</th>
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
During **Migration Assessment**, a team of system architects does the following:
- review the architecture of the existing application
- produce an assessment report that includes a network diagram with all the application layers
- identify the application and database components that are not automatically migrated
- estimate the effort for manual conversion work

The SCT's [database migration assessment report](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_AssessmentReport.html) tool can be used to summarize all of the schema conversion tasks, and details [Action Items](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_AssessmentReport.ActionItems.html) (and their complexity) for schema that can't be converted to the target database engine. It can also provide recommendations for the best target engine, other AWS services can fill in for missing features, and unique RDS features that can save customer licensing and other costs.

Using the detailed report provided by the SCT, system architects can provide a much more precise estimate for the efforts required to complete migration of the database schema code.

## Step 2: Schema Conversion
The **Schema Conversion** step consists of translating the data definition language (DDL) for tables, partitions, and other database storage objects from the source database to the target database. In the SCT, this is a two-step process:
1. Convert the schema.
2. Apply the schema to the target database.

The SCT automatically creates DDL scripts for as many database objects on the target platform as possible. For the remaining objects, the conversion Action Items describe why the object cannot be converted automatically and the manual steps required to convert the object to the target platform.

The SCT also allows for the user to create custom schema tranformations and mapping rules to use during the conversion that are applied to the target platform. Some of the transformations include:
- Rename
- Add prefix
- Add suffix
- Remove prefix
- Remove suffix
- Convert uppercase
- Convert lowercase
- Change data type (columns only)

## Step 3: Embedded SQL and Application Code Conversion
After converting the schema, the next step is to address any custom scripts with embedded SQL statements (ETL scripts, reports, etc.) and the application code so that they work with the target database. This includes rewriting parts of the code that relate to JDBC/ODBC driver usage, establishing connections, data retrieval, and iteration.

The SCT scans a folder containing application code, extracts embedded SQL statements, converts as many as possible automatically, and flags the remaining statements for manual conversion actions. The workflow for application code conversion is similar to the workflow for the database migration:
1. Run an assessment report to understand the level of effort required to convert the application code to the target platform.
2. Analyze the code to extract embedded SQL statements.
3. Allow the SCT to automatically convert as much code as possible.
4. Work through the remaining conversion Action Items manually.
5. Save code changes.

## Step 4: Data Migration
After schema and application code conversion, the next step is to migrate the data from the source database to the target database. This can be easily accomplished by using the DMS. The DMS works by setting up a replication server on an EC2 instance called a **replication instance**, and it acts as a middleman between the source and the target.

There are three main steps to start the data migration service:
1. Set up a replication instance in a VPC (that can connect to both the source and target databases).
2. Define connections for the source and target databases.
3. Define data replication tasks.

Data can be migrated in two ways:
- As a full load of existing data
- As a full load of existing data, followed by continuous replication of data changes to the target

## Step 5: Testing Converted Code
Thorough testing of the migrated application ensures correct functional behaviour on the new platform. It should accomplish two goals:
1. Verify critical functionality of the application.
2. Verify that converted SQL objects are functioning as intended.

This should be given the same effort as in both the conversion phases, up to 45% of the total migration effort.

## Step 6: Data Replication
Many production applications cannot tolerate a downtime window long enough to migrate all the data in a full load. The DMS can use a proprietary Change Data Capture (CDC) process to implement ongoing replication from the source database to the target database, with minimal load on the source.

The CDC offers two ways to implement ongoing replication:
1. Migrate existing data, and replicate ongoing changes. Replication is done by:
   1. Migrating existing data and caching changes to it
   2. Applying the cached data changes until the database reaches a steady state
   3. Applying current data changes to the target as soon as they're received by the replication instance
2. Replicate data changes only.
    - This option is helpful when the target schema already existins and the intial data load is already completed.

## Step 7: Go Live (Deployment to AWS)
Test the data migration to ensure that all data can be successfully migrated during the cutover window. Monitor the source and target databases to ensure that: 
- The initial data load is completed
- Cached transactions are applied
- The data has reached a steady state before cutover

DMS monitors the numbers of rows inserted, deleted, and updated, as well as the number of DDL statements issued per table while a task is running. DMS also publishes metrics to [Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html), which can be used for post-deployment monitoring and monitoring possible DMS errors.

# References
- [Whitepaper](https://d1.awsstatic.com/whitepapers/Migration/migrating-applications-to-aws.pdf)
- [AWS Schema Conversion Tool User Guide](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html)

