# Solution Overview<a name="oracle-s3-data-lake-solution-overview"></a>

The following diagram shows the architecture of a migration from RDS for Oracle to Amazon S3 using AWS DMS\.

![\[An architecture diagram of the migration from Oracle to an Amazon S3 data lake.\]](http://docs.aws.amazon.com/dms/latest/sbs/images/oracle-s3-data-lake-architecture-diagram.png)

The Amazon RDS for Oracle database contains the example sales history data set\. AWS Database Migration Service \(AWS DMS\) contains several components used to host the replication engine\. Amazon S3 provides storage for the data lake tables and downstream applications for machine learning and analytics consume the data lake information\.

To run this wakkthrough, create the following resources in AWS DMS\.
+ Replication instance — An AWS managed instance that hosts the AWS DMS engine\. You control the type or size of the instance based on the workload you plan to migrate\.
+ Source endpoint — An endpoint that provides connection details, data store type, and credentials to connect to a source database\. For this use case, we configure the source endpoint to point to the Amazon RDS for Oracle database\.
+ Target endpoint — AWS DMS supports several target systems including Amazon RDS, Amazon Aurora, Amazon Redshift, Amazon Kinesis Data Streams, Amazon S3, and so on\. For this use case, we configure Amazon S3 as the target endpoint\.
+ Replication task — A task that runs on the replication instance and connects to endpoints to replicate data from the source database to the target database\.

In the rest of this document, we show how to configure each of these components to migrate the sales history data set\. We start with the prerequisites to complete this walkthrough, and then continue with the step\-by\-step migration procedure and conclusion\.