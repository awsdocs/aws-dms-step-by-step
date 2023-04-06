# Solution Overview<a name="chap-rdssqlserver2s3datalake.overview"></a>

The following diagram displays a high\-level architecture of the solution, where we use AWS DMS to move data from Microsoft SQL Server databases hosted on Amazon Relational Database Service \(Amazon RDS\) to Amazon Simple Storage Service \(Amazon S3\)\.

![\[A high-level architecture diagram of the migration solution.\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdssqlserver2s3datalake-solution-overview.png)

The following diagram shows the structure of the Amazon S3 bucket from the preceding diagram\.

![\[bucket structure.\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdssqlserver2s3datalake-s3-bucket-structure.png)

To replicate data, you need to create and configure the following artifacts in AWS DMS:
+  **Replication Instance** — An AWS managed instance that hosts the AWS DMS engine\. You control the type or size of the instance based on the workload you plan to migrate\.
+  **Source Endpoint** — An endpoint that provides connection details, data store type, and credentials to connect to a source database\. For this use case, we will configure the source endpoint to point to the Amazon RDS for SQL Server database\.
+  **Target Endpoint** — AWS DMS supports several target systems including Amazon RDS, Amazon Aurora, Amazon Redshift, Amazon Kinesis Data Streams, Amazon S3, and more\. For the use case, we will configure Amazon S3 as the target endpoint\.
+  **Replication Task** — A task that runs on the replication instance and connects to endpoints to replicate data from the source database to the target database

For this walkthrough, we will use the `AdventureWorks` sample database on an Amazon RDS for SQL Server instance as the base data for the walkthrough\. The `AdventureWorks` database holds sales, marketing, and order data\. We will use AWS DMS to move sales data from the source database to Amazon S3 object store, which can be used as a data lake for downstream analytics needs\.

**Note**  
You can refer to [Migrating a SQL Server Always On Database to Amazon Web Services](chap-manageddatabases.sqlserveralwayson.md) for details on migrating from a Microsoft SQL Server Always On database instance\.

We will create an AWS DMS task, which will perform a one\-time full load to migrate a point in time snapshot and will then stream incremental data to the target Amazon S3 bucket\. This way, sales data in the S3 bucket will be kept in sync with the source database\.