# \[\.shared\]`DMS` Ongoing Replication<a name="chap-manageddatabases.mysql2rds.replication"></a>

To configure the ongoing replication in AWS DMS, enter the native start point for MySQL, which you have retrieved at the end of the full load process as described for each tool\. The native start point will be similar to `mysql-bin-changelog.000024:373`\.

In the **Create database migration task** page, follow these three steps to create the migration task\.

1. For **Migration type**, choose **Replicate ongoing changes**\.

1. Under **CDC start mode for source transactions**, choose **Enable custom CDC start mode**\.

1. Under **Custom CDC start point**, paste the native start point you saved earlier\.

For more information, see [Creating tasks for ongoing replication using AWS DMS](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Task.CDC.html) and [Migrate from MySQL to Amazon RDS with AWS DMS](https://aws.amazon.com/getting-started/hands-on/move-to-managed/migrate-my-sql-to-amazon-rds/)\.

**Note**  
The AWS DMS CDC replication uses plain SQL statements from binlog to apply data changes in the target database\. Therefore, it is slower and more resource\-intensive than the native Primary/Replica binary log replication in MySQL\. For more information, see [Replication with a MySQL or MariaDB instance running external to Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Procedural.Importing.External.Repl.html)\.

You should always remove triggers from the target during the AWS DMS CDC replication\. For example, the following command generates the script to remove triggers\.

```
# In case required to generate drop triggers script
SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM information_schema.TRIGGERS WHERE TRIGGER_SCHEMA not in ('sys','mysql');
```