# Step 2: Configure a Source Amazon RDS for SQL Server Database<a name="chap-rdssqlserver2s3datalake.steps.configuresource"></a>

One of the primary considerations when setting up AWS DMS replication is the load that it induces on the source database\. During full load, AWS DMS tasks initiate two or three connections for each table that is configured for parallel load\. Because AWS DMS settings and data volumes vary across tasks, workloads, and even across different runs of the same task, providing an estimate of resource utilization that applies for all use cases is difficult\.

Ongoing replication is single\-threaded and it usually consumes less resources than full load\. Providing estimates for change data capture \(CDC\) resource utilization has the same challenges described before\.

That said, you can estimate the expected increase in load on your source Amazon RDS instance, by running test AWS DMS tasks on replicas of your source Amazon RDS for SQL Server instance and monitoring the CPU, memory, IO and throughput metrics\.

For our source database, we use an `m5.xlarge` Amazon RDS instance running Microsoft SQL Server 2019\. While the steps for Amazon RDS for SQL Server creation are out of scope for this walkthrough \(for more information, see [Prerequisites](chap-rdssqlserver2s3datalake.prerequisites.md)\), make sure that your Amazon RDS instance has **Automatic Backups** turned on so that the recovery model for the database is set to **FULL**\. This is a pre\-requisite for ongoing replication with AWS DMS\. You can turn on these settings when you create or modify an existing Amazon RDS instance\.

The following image displays the database settings required for ongoing replication with AWS DMS\.

![\[Database backup settings required for ongoing replication.\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdssqlserver2s3datalake-backup-settings.png)

To perform the full load phase, AWS DMS requires read privileges to the tables in scope for migration\. For more information about required permissions, see [Permissions for full load only tasks](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html#CHAP_Source.SQLServer.Permissions)\.

Connect to the Amazon RDS for SQL Server instance and run the following queries\. Use a login with master user privileges for both full load and CDC\.

```
USE AdventureWorks;
CREATE LOGIN dms_user WITH PASSWORD = 'password'
CREATE USER dms_user FOR LOGIN dms_user
ALTER ROLE [db_datareader] ADD MEMBER dms_user
ALTER ROLE [db_owner] ADD MEMBER dms_user
GRANT VIEW DATABASE STATE to dms_user

USE master;
GRANT VIEW SERVER STATE TO dms_user
```

**Note**  
Here, we create a new user to perform the migration\. You can skip this step if you plan to use existing logins and users that have the required privileges\.

Turn on MS\-CDC for your Amazon RDS for SQL Server database instance at the database level\.

```
exec msdb.dbo.rds_cdc_enable_db 'AdventureWorks'
```

Because we migrate all tables in the `Sales` schema of the `AdventureWorks` database, we need to identify the total number of tables\.

```
SELECT TABLE_SCHEMA, TABLE_NAME, TABLE_TYPE
FROM information_schema.tables
WHERE TABLE_SCHEMA = 'Sales'
ORDER BY TABLE_NAME
```

Then we need to divide tables in the following groups:
+ Tables with a primary key\.
+ Tables with a unique index without primary key\.
+ Tables without a primary key and unique index\.

We use the information\_schema to identify tables that have a primary key or a unique index without a primary key\.

```
SELECT a.TABLE_SCHEMA, a.TABLE_NAME, a.CONSTRAINT_TYPE, CONSTRAINT_NAME
FROM information_schema.table_constraints a
JOIN information_schema.tables b ON aTABLE_SCHEMA = b.TABLE_SCHEMA
AND a.TABLE_NAME = b.TABLE_NAME
WHERE b.TABLE_TYPE = 'BASE TABLE'
AND a.TABLE_SCHEMA = 'Sales'
AND a.CONSTRAINT_TYPE in ('UNIQUE','PRIMARY KEY')
ORDER BY a.TABLE_SCHEMA, a.TABLE_NAME
```

The query results show that the task has 19 tables and all of them have primary keys\. For all these tables, run the following query to turn on MS\-CDC at the table level\.

```
exec sys.sp_cdc_enable_table
@source_schema = N'Sales',
@source_name = N'table_name',
@role_name = NULL,
@supports_net_changes = 1
```

Now, set the retention period for changes to be available on the source using the following commands\. Set the `pollinginterval` value to 86399 seconds to increase the retention of changes on the Amazon RDS for SQL Server instance\.

```
EXEC sys.sp_cdc_change_job @job_type = 'capture' ,@pollinginterval = 86399
exec sp_cdc_stop_job 'capture'
exec sp_cdc_start_job 'capture'
exec sys.sp_cdc_help_jobs
```

Set the polling interval on your secondary database to 86399 seconds too\. For most use cases these settings should be enough\. For databases that have a large number of transactions, you need to make additional configuration changes to make sure that the transaction log has optimal retention\. For more information, see [Optional settings when using Amazon RDS for SQL Server as a source](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html#CHAP_Source.SQLServer.OptionalSettings)\.

For more information about ongoing replication, see [Setting up ongoing replication on a Cloud SQL Server DB instance](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html#CHAP_Source.SQLServer.Configuration)\.

**Note**  
 AWS DMS does not support replicating ongoing changes from views\. For more information, see [Selection rules and actions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Selections.html)\.

In this walkthrough, we focus on migrating the tables and do not include views in the migration scope\. You should also look at estimating the number of records in the tables you are going to migrate as this is a useful consideration while configuring AWS DMS tasks\.