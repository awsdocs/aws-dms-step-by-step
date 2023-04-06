# Backup and Restore Using Amazon S3<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-backup-restore"></a>

Backup and restore is the easiest and usually the preferred method for the initial load of the target database\. In this method, you create a full backup of your self\-managed SQL Server database, transfer it to an Amazon S3 bucket, and restore it to your Amazon RDS for SQL Server instance\. For more information, see [Importing and exporting SQL Server databases using native backup and restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html) in the *Amazon RDS User Guide*\.

The backup and restore method is suitable for the following use cases:
+ Your database size is less than 16 TB\.
+ You want to carry out a lift and shift migration with no changes or minimal changes to the database\. For example, you want to migrate secondary database objects such as users, views, stored procedures, triggers, and so on in addition to your data\.
+ Network connectivity between your on\-premises data center and AWS is often congested or has frequent disconnects\. Backup and restore gives you the flexibility to transmit backup files during non\-business hours\.

The backup and restore method has the following limitations:
+ Amazon RDS for SQL Server supports native restore of databases up to 16 TB in size\. For SQL Server Express Edition databases, Amazon RDS supports native restore of up to 10 GB\.
+ On Multi\-AZ database instances, you can only natively restore databases that are backed up in full recovery model\.
+ The Amazon S3 bucket where you store your data, has to be located in the same AWS Region as your target Amazon RDS for SQL Server database instance\.
+ Restoring backups from one time zone to a different time zone isn’t recommended\.
+ You can’t transform or filter data at a table\-level when you use backup and restore\.
+ When you need to migrate a subset of tables, you can’t use backup and restore\.

## Migration Steps<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-backup-restore-steps"></a>

At a high level, the steps involved in backup and restore are the following:
+ Perform a full backup of the source database\.
+ Copy the backup file to an Amazon S3 bucket\.
+ Restore the backup from the Amazon S3 bucket onto the target Amazon RDS for SQL Server database\.

We use the [https://github.com/aws-samples/aws-database-migration-samples/blob/master/sqlserver/sampledb/v1/README.md](https://github.com/aws-samples/aws-database-migration-samples/blob/master/sqlserver/sampledb/v1/README.md) database in the following example\.

## Perform Full Backup<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-backup-restore-full-backup"></a>

First, perform a full back up of the source database\. Amazon S3 currently limits data files to 5 TB\. If the database backup size is less than 5 TB, you can use the following command\.

```
Use [dms_sample]
GO

BACKUP DATABASE [dms_sample] TO
DISK = 'C:\Backup\dms_sample.bak'
WITH NOFORMAT, NOINIT,
NAME = 'Full Backup of dms_sample', SKIP, NOREWIND, NOUNLOAD, STATS = 10
Go
```

If your database is larger than 5 TB, split the backup files\. Make sure that each file is less than 5 TB in size\. For example, the size of the `dms_sample` database is 15 TB\. This means that we use three backup files\.

```
Use [dms_sample]
GO

BACKUP DATABASE [dms_sample] TO
DISK = 'C:\Backup\dms_sample1.bak',
DISK = 'C:\Backup\dms_sample2.bak',
DISK = 'C:\Backup\dms_sample3.bak'
WITH NOFORMAT, NOINIT,
NAME = 'Full Backup of dms_sample', SKIP, NOREWIND, NOUNLOAD, STATS = 10
Go
```

## Copy Backup Files to Amazon S3<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-backup-restore-copy-backup"></a>

Now, use the AWS CLI to upload the backup file to an Amazon S3 bucket\.

```
aws s3 cp C:\Backup\dms_sample.bak s3://sampledatabaseuswest2/
```

For multiple backup files, use the folder path to copy the backup files to an Amazon S3 bucket\.

```
aws s3 cp "C:\Backup" s3://sampledatabaseuswest2/ --recursive
```

Make sure that you define an AWS Identity and Access Management \(IAM\) role to access the option group\. An option group can specify features, called options, that are available for a particular Amazon RDS DB instance\. When you associate a DB instance with an option group, the specified options and option settings are enabled for that DB instance

When you create this IAM role, attach a trust relationship and a permissions policy\. For more information, see [Manually creating an IAM role for native backup and restore](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html#SQLServer.Procedural.Importing.Native.Enabling.IAM)\.

We create the `sql-server-backup-restore` role, and then use it when we configure the target Amazon RDS database\.

## Restore Your Backup to the Target Database<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-backup-restore-restore-backup"></a>

To restore your backup, do the following:

1. Create an option group for the target database\.

   1. In the Amazon RDS console, choose **Option groups**, and then choose **Create option group**\.

   1. For **Name**, enter **SQLServerrestore**\.

   1. For **Description**, enter **SQLServerrestore**\.

   1. For **Engine**, choose **sqlserver\-se**\.

   1. For **Major engine version**, choose **14\.00**\.

   1. Choose **Create**\.

1. Add the `SQLSERVER_BACKUP_RESTORE` option and the `sql-server-backup-restore` role to this option group to access S3 bucket\.

   1. On the **Option groups** page, choose the option group that you created\.

   1. For **Options**, choose **Add option**\. The **Add option** page opens\.

   1. For **Option name**, choose **SQLSERVER\_BACKUP\_RESTORE**\.

   1. For **IAM role**, choose the `sql-server-backup-restore` role\.

1. Modify your Amazon RDS for SQL Server DB instance and attach this option group\.

   1. In the Amazon RDS console, choose **Databases**, and then choose your target database\.

   1. Choose **Modify**\. The **Modify DB instance** page opens\.

   1. In the **Additional configuration** section, choose **SQLServerrestore** for **Option group**\.

Now, you can restore the backup file from Amazon S3 into the target Amazon RDS for SQL Server database\. To restore your database, call the `rds_restore_database` stored procedure\. For more information, see [Restoring a database](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/SQLServer.Procedural.Importing.html#SQLServer.Procedural.Importing.Native.Using.Restore)\.

```
exec msdb.dbo.rds_restore_database
@restore_db_name='DMS',
@s3_arn_to_restore_from='arn:aws:s3:::sampledatabaseuswest2/dms_sample.bak';
```

To restore multiple backup files, use the following command\.

```
exec msdb.dbo.rds_restore_database
@restore_db_name='DMS',
@s3_arn_to_restore_from='arn:aws:s3:::sampledatabaseuswest2/dms_sample*';
```

The preceding statement returns the ID of the task\. You can use the following command to check the status of the restore using this task ID\.

```
exec msdb.dbo.rds_task_status
 [@db_name='DMS'],
 [@task_id=<ID_number>];
```

Finally, use the following SQL command to get the log sequence number \(LSN\) of the on\-premises source database backup\. Then use this LSN to set up the change data capture \(CDC\) task in AWS DMS\.

```
Use [dms_sample]
GO

SELECT [Current LSN], [Begin Time], Description FROM fn_dblog(NULL, NULL) Where [Transaction Name] = 'Backup:CommitDifferentialBase'
```