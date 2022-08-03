# Migrating an Oracle Database to Amazon RDS for Oracle<a name="chap-manageddatabases.oracle2rds"></a>

You can use these three main approaches to migrate self\-managed Oracle databases to Amazon Relational Database Service \(Amazon RDS\) for Oracle\.
+ Using a native database tool such as Oracle Data Pump\.
+ Using a managed service such as the AWS Database Migration Service \(AWS DMS\)\.
+ Using a native tool for the full load phase and AWS DMS for ongoing replication\.

This document describes the third strategy — we call this the hybrid approach\. The following diagram shows the components of the hybrid approach\.

![\[Hybrid migration approach\]](http://docs.aws.amazon.com/dms/latest/sbs/images/oracle2rds-migration-approach.png)

The hybrid approach may be appropriate in the following circumstances\.
+ To automate the creation of secondary database objects such as views, indexes, and constraints\.
+ To use AWS DMS data validation to ensure your target data matches with the source, row by row and column by column\.
+ To use some of the other capabilities that AWS DMS provides, for example data filtering or renaming tables and columns\.

This document describes the native options for the full load\. It also includes a comparison so you can evaluate the options against your migration requirements\. In conclusion, you can find a brief description of how to use AWS DMS for ongoing replication\.

**Topics**
+ [Full Load](chap-manageddatabases.oracle2rds.load.md)
+ [Performance Comparison](chap-manageddatabases.oracle2rds.performance.md)
+ [Ongoing Replication](chap-manageddatabases.oracle2rds.replication.md)
+ [Summary](chap-manageddatabases.oracle2rds.summary.md)
+ [Resources](chap-manageddatabases.oracle2rds.resources.md)