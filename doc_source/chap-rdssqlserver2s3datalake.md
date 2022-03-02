# Migrating an Amazon RDS for SQL Server Database to an Amazon S3 Data Lake<a name="chap-rdssqlserver2s3datalake"></a>

This walkthrough gets you started with the process of migrating from an Amazon Relational Database Service \(Amazon RDS\) for Microsoft SQL Server to Amazon Simple Storage Service \(Amazon S3\) cloud data lake using AWS Database Migration Service \(AWS DMS\)\.

For most organizations, data is distributed across multiple systems and data stores to support varied business needs\. On\-premises data stores struggle to scale performance as data sizes and formats grow exponentially for analytics and reporting purposes\. These limitations in data storage and management limit efficient and comprehensive analytics\.

 Amazon S3 based data lakes provide reliable and scalable storage, where you can store structured, semi\-structured and unstructured datasets for varying analytics needs\. You can integrate Amazon S3 based data lakes with distributed processing frameworks such as Apache Spark, Apache Hive, and Presto to decouple compute and storage, so that both can scale independently\.

**Topics**
+ [Why Amazon S3?](#chap-rdssqlserver2s3datalake.whys3)
+ [Why AWS DMS?](#chap-rdssqlserver2s3datalake.whydms)
+ [Solution Overview](chap-rdssqlserver2s3datalake.overview.md)
+ [Prerequisites](chap-rdssqlserver2s3datalake.prerequisites.md)
+ [Step\-by\-Step Migration](chap-rdssqlserver2s3datalake.steps.md)

## Why Amazon S3?<a name="chap-rdssqlserver2s3datalake.whys3"></a>

 Amazon S3 is an object storage service for structured, semi\-structured, and unstructured data that offers industry\-leading scalability, data availability, security, and performance\. With a data lake built on Amazon S3, you can use native AWS services, optimize costs, organize data, and configure fine\-tuned access controls to meet specific business, organizational, and compliance requirements\.

 Amazon S3 is designed for 99\.999999999% \(11 9s\) of data durability\. The service automatically creates and stores copies of all uploaded S3 objects across multiple systems\. This means your data is available when needed and protected against failures, errors, and threats\.

 Amazon S3 is secure by design, scalable on demand, and durable against the failure of an entire AWS Availability Zone\. You can use AWS native services and integrate with third\-party service providers to run applications on your data lake\.

## Why AWS DMS?<a name="chap-rdssqlserver2s3datalake.whydms"></a>

Data lakes typically require building, configuring, and maintaining multiple data ingestion pipelines from cloud and on\-premises data stores\.

Traditionally, databases can be loaded once with data ingestion tools such as import, export, bulk copy, and so on\. Ongoing changes are either not possible or are implemented by bookmarking the initial state\. Setting up a data lake using these methods can present challenges ranging from increased load on the source database to overheads while carrying schema changes\.

 AWS DMS supports a one\-time load and near\-real\-time ongoing replication making the data migration seamless, while supporting multiple source and target database platforms\. One of the common use cases is the need to derive insights on data stored in several sources\. For example, you may need to identify monthly sales for a specific year on sales data stored on different database instances\.

As a part of this walkthrough, we will configure AWS DMS to move data from an Amazon RDS for SQL Server database instance to Amazon S3 for a sales analytics use case\.

**Note**  
This introductory exercise doesn’t cover all use cases of migrating to Amazon S3 but provides an overview of the migration process using AWS DMS\. This example covers commonly faced problems and describes best practices to follow when migrating to an Amazon S3 data lake\.