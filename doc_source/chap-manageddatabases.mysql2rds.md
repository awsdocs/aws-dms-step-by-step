# Migrating a MySQL Database to RDS for MySQL or Aurora MySQL<a name="chap-manageddatabases.mysql2rds"></a>

You can use these two main approaches for migrating a self\-managed MySQL database to an Amazon RDS for MySQL or Amazon Aurora MySQL database\.
+ Use a native or third\-party database migration tool such as mysqldump to perform the full load and MySQL replication to perform ongoing replication\. Typically this is the simplest option\.
+ Use a managed migration service such as the AWS Database Migration Service \(AWS DMS\)\. AWS DMS provides migration\-specific services such as data validation that are not available in the native or third\-party tools\.

The following diagram displays these two approaches\.

![\[Different approaches to MySQL database migration to Amazon RDS for MySQL\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-mysql2rds-migration-approaches.png)

You can use a hybrid strategy that combines native or third\-party tools for full load and AWS DMS for ongoing replication\. The following diagram displays the hybrid migration approach\.

![\[Hybrid migration approach to MySQL database migration to Amazon RDS for MySQL\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-mysql2rds-hybrid-migration-approach.png)

The hybrid option delivers the simplicity of the native or third\-party tools along with the additional services that AWS DMS provides\. For example, in AWS DMS, you can automatically validate your migrated data, row by row and column by column, to ensure the data quality in the target database\. Or, if you are only migrating a subset of the tables, it will be simpler to use AWS DMS to filter your tables than the equivalent configuration in the native or third\-party tools\.

**Topics**
+ [Full Load](chap-manageddatabases.mysql2rds.fullload.md)
+ [Performance Comparison](chap-manageddatabases.mysql2rds.performance.md)
+ [AWS DMS Ongoing Replication](chap-manageddatabases.mysql2rds.replication.md)
+ [Resources](chap-manageddatabases.mysql2rds.resources.md)