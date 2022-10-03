# Migrating SQL Server Databases to Amazon RDS for SQL Server<a name="chap-manageddatabases.sql-server-rds-sql-server"></a>

This walkthrough gets you started with homogeneous database migration from Microsoft SQL Server to Amazon Relational Database Service \(Amazon RDS\) for SQL Server\. This guide provides a quick overview of the data migration process and provides suggestions on how to select the best option to use\.

Customers looking to migrate self\-managed SQL Server databases to Amazon RDS for SQL Server, can use one of the three main approaches\.
+ Use a native database migration method such as backup and restore\.
+ Use a managed service such as AWS DMS\.
+ Use a native tool for full load and a managed AWS DMS service for ongoing replication\. We call this strategy the *hybrid approach*\.

The following diagram shows the hybrid approach\. Here, we use one of the three native tools for full load, and AWS DMS for ongoing replication\.

![\[Hybrid approach for SQL Server to Amazon RDS for SQL Server migration\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sql-server-rds-sql-server-hybrid-migration.png)

The hybrid approach provides the simplicity of the native tools with additional built\-in capabilities of AWS DMS\. These include:
+ Data validation
+ Customizable source object selection rules
+ Data filtering
+ Renaming target tables or columns
+ Data transformations
+ Data partitioning

This document describes in detail the three full load migration methods\. This guide helps you evaluate each method for your migration requirements\. In the end, you can find a brief description of how to use AWS DMS for ongoing replication\.

**Topics**
+ [Full Load](chap-manageddatabases.sql-server-rds-sql-server-full-load.md)
+ [Performance Comparison](chap-manageddatabases.sql-server-rds-sql-server-performance.md)
+ [Ongoing Replication](chap-manageddatabases.sql-server-rds-sql-server-replication.md)
+ [Summary](chap-manageddatabases.sql-server-rds-sql-server-summary.md)
+ [Resources](chap-manageddatabases.sql-server-rds-sql-server-resources.md)