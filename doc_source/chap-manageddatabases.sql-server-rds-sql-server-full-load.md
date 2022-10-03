# Full Load<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load"></a>

The full load migration phase populates the target database with a copy of the source data\. In each section, you can find detailed information about the full load method and their results to help you choose the one that fits your use case\. For all three methods, we use the [https://github.com/aws-samples/aws-database-migration-samples/blob/master/sqlserver/sampledb/v1/README.md](https://github.com/aws-samples/aws-database-migration-samples/blob/master/sqlserver/sampledb/v1/README.md) database as an example\. The `dms_sample` database includes tables, views, indexes, stored procedures, and other database objects\.

**Topics**
+ [Backup and Restore Using Amazon S3](chap-manageddatabases.sql-server-rds-sql-server-full-load-backup-restore.md)
+ [SQL Server Import and Export Wizard](chap-manageddatabases.sql-server-rds-sql-server-full-load-import-export.md)
+ [Generate and Publish Scripts Wizard and Bulk Copy Program Utility](chap-manageddatabases.sql-server-rds-sql-server-full-load-bcp.md)