# Migrating PostgreSQL Databases to Amazon RDS for PostgreSQL or Amazon Aurora PostgreSQL<a name="chap-manageddatabases.postgresql-rds-postgresql"></a>

This walkthrough gets you started with homogeneous database migration from PostgreSQL to Amazon Relational Database Service \(Amazon RDS\) for PostgreSQL or Amazon Aurora PostgreSQL\-Compatible Edition\. This guide provides a quick overview of the data migration process and provides suggestions on how to select the best option to use\.

Customers looking to migrate self\-managed PostgreSQL databases to Amazon RDS for PostgreSQL or Aurora PostgreSQL, can use one of the three main approaches\.
+ Use a native or third\-party database migration method such as pg\_dump and pg\_restore for full load only migrations\.
+ Use a managed service such as AWS Database Migration Service \(AWS DMS\) for full load and ongoing replication\.
+ Use a native tool for full load and a managed AWS DMS service for ongoing replication\. We call this strategy the *hybrid approach*\.

This document describes the hybrid approach\. The following diagram shows the components of the hybrid approach\.

![\[PostgreSQL Databases to Amazon RDS for PostgreSQL hybrid migration approach\]](http://docs.aws.amazon.com/dms/latest/sbs/images/postgresql-rds-postgresql-migration-approach.png)

The hybrid approach provides the following advantages\.
+ Automation of the creation of secondary database objects such as views, indexes, and constraints\.
+  AWS DMS data validation to ensure that your target data matches with the source, row by row and column by column\.
+ Other capabilities provided by AWS DMS, for example CloudWatch monitoring and table statistics\. It may be simpler to use AWS DMS to track migration progress, transactional workload, receive and transmit throughput, source and target latency, and so on\.

This document describes the native options for the full load\. It also includes a comparison so that you can evaluate the options for your migration requirements\. In conclusion, you can find a brief description of how to use AWS DMS for ongoing replication\.

**Topics**
+ [Full Load](chap-manageddatabases.postgresql-rds-postgresql-full-load.md)
+ [Performance Comparison](chap-manageddatabases.postgresql-rds-postgresql-performance-comparison.md)
+ [Ongoing Replication](chap-manageddatabases.postgresql-rds-postgresql-ongoing-replication.md)
+ [Summary](chap-manageddatabases.postgresql-rds-postgresql-summary.md)
+ [Resources](chap-manageddatabases.postgresql-rds-postgresql-resources.md)