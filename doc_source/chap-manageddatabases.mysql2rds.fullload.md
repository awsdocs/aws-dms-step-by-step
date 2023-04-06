# Full Load<a name="chap-manageddatabases.mysql2rds.fullload"></a>

You can use one of these three tools to move data from your MySQL database to Amazon RDS for MySQL or Amazon Aurora MySQL\. Follow the steps described in this document to perform the full data load\.

## mysqldump<a name="chap-manageddatabases.mysql2rds.fullload.mysqldump"></a>

This native MySQL client utility installs by default with the engine that performs logical backups, producing a set of SQL statements that you can execute to reproduce the original database object definitions and table data\. mysqldump dumps one or more MySQL databases for backup or transfer to another MySQL server\. For more information, see the [mysqldump documentation](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)\.

mysqldump is appropriate when the following conditions are met:
+ The data set is smaller than 10 GB\.
+ The network connection between source and target databases is fast and stable\.
+ Migration time is not critical, and the cost of re\-trying the migration is very low\.
+ You don’t need to do any intermediate schema or data transformations\.

You can decide not to use this tool if any of the following conditions are true:
+ You migrate from an Amazon RDS for MySQL DB instance or a self\-managed MySQL 5\.5 or 5\.6 database\. In that case, you can get better performance results with Percona XtraBackup\.
+ It is impossible to establish a network connection from a single client instance to source and target databases due to network architecture or security considerations\.
+ The network connection between the source and target databases is unstable or very slow\.
+ The data set is larger than 10 GB\.
+ An intermediate dump file is required to perform schema or data manipulations before you can import the schema or data\.

For details and step\-by\-step instructions, see [Importing data to an Amazon RDS for MySQL or MariaDB DB instance with reduced downtime](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Procedural.Importing.NonRDSRepl.html#MySQL.Procedural.Importing.Database.Backup.Procedure) in the *Amazon RDS User Guide*\.

Follow these three steps to perform full data load using mysqldump\.

1. Produce a dump file containing source data\.

1. Restore this dump file on the target database\.

1. Retrieve the binary log position for ongoing replication\.

For example, the following command creates the dump file\. The `--master-data=2` parameter creates a backup file, which you can use to start the replication in AWS DMS\.

```
sudo mysqldump \
    --databases <database_name> \
    --master-data=2  \
    --single-transaction \
    --order-by-primary \
    -r <backup_file>.sql \
    -u local_user \
    -p <local_password>
```

For example, the following command restores the dump file on the target host\.

```
mysql -h host_name -P 3306 -u db_master_user -p < backup_file.sql
```

For example, the following command retrieves the binary log file name and position from the dump file\. Save this information for later when you configure AWS DMS for ongoing replication\.

```
head mysqldump.sql -n80 | grep "MASTER_LOG_POS"

-- Will Get output similar to
-- CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000125', MASTER_LOG_POS=150;
```

## Percona XtraBackup<a name="chap-manageddatabases.mysql2rds.fullload.percona"></a>

 Amazon RDS for MySQL and Amazon Aurora MySQL support migration from Percona XtraBackup files that are stored in an Amazon S3 bucket\. Percona XtraBackup produces a binary backup files which can be significantly faster than migrating from logical schema and data dumps using tools such as mysqldump\. The tool can be used for small\-scale to large\-scale migrations\.

Percona XtraBackup is appropriate when the following conditions are met:
+ You have administrative, system\-level access to the source database\.
+ You migrate database servers in a 1\-to\-1 fashion: one source MySQL server becomes one new Amazon RDS for MySQL or Aurora DB cluster\.

You can decide not to use this tool if any of the following conditions are true:
+ You can’t use third\-party software because of operating system limitations\.
+ You migrate into existing Aurora DB clusters\.
+ You migrate multiple source MySQL servers into a single Aurora DB cluster\.
+ For more information, see [Limitations and recommendations for importing backup files from Amazon S3 to Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/MySQL.Procedural.Importing.html#MySQL.Procedural.Importing.Limitations)\.

For details and step\-by\-step instructions, see [Migrating data from MySQL by using an Amazon S3 Bucket](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Migrating.ExtMySQL.html#AuroraMySQL.Migrating.ExtMySQL.S3) in the *Amazon RDS User Guide*\.

Follow these three steps to perform full data load using Percona XtraBackup\.

1. Produce a backup file containing source data\.

1. Restore this backup file from Amazon S3 while launching a new target database\.

1. Retrieve the binary log position for ongoing replication\.

For example, the following command creates the backup file and streams it directly to Amazon S3\.

```
xtrabackup --user=<myuser> --backup --parallel=4 \
--stream=xbstream --compress | \
aws s3 cp - s3://<bucket_name>/<backup_file>.xbstream
```

Use the Amazon RDS console to restore the backup files from the Amazon S3 bucket and create a new Amazon Aurora MySQL DB cluster\. For more information, see [Restoring an Aurora MySQL DB cluster from an Amazon S3 bucket](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Migrating.ExtMySQL.html#AuroraMySQL.Migrating.ExtMySQL.S3.Restore)\.

For example, the following command prints the binary log \(binlog\) information after you finish the creation of a compressed backup\.

```
MySQL binlog position: filename 'mysql-bin.000001', position '481'
```

For example, the following command retrieves the binary log file name and position from the from the xtrabackup\_binlog\_info file\. This file is located in the main backup directory of an uncompressed backup\.

```
$ cat </on-premises/backup>/xtrabackup_binlog_info
// Output
mysql-bin.000001     481
```

## mydumper<a name="chap-manageddatabases.mysql2rds.fullload.mydumper"></a>

mydumper and myloader are third\-party utilities that perform a multithreaded schema and data migration without the need to manually invoke any SQL commands or design custom migration scripts\. mydumper functions similarly to mysqldump, but offers many improvements such as parallel backups, consistent reads, and built\-in compression\. Another benefit to mydumper is that each individual table gets dumped into a separate file\. The tools are highly flexible and have reasonable configuration defaults\. You can adjust the default configuration to satisfy the requirements of both small\-scale and large\-scale migrations\.

mydumper is appropriate when the following conditions are met:
+ Migration time is critical\.
+ You can’t use Percona XtraBackup\.

You can decide not to use this tool if any of the following conditions are true:
+ You migrate from an Amazon RDS for MySQL DB instance or a self\-managed MySQL 5\.5 or 5\.6 database\. In that case, you might get better results Percona XtraBackup\.
+ You can’t use third\-party software because of operating system limitations\.
+ Your data transformation processes require intermediate dump files in a flat\-file format and not an SQL format\.

For details and step\-by\-step instructions, see the [mydumper project](https://github.com/maxbube/mydumper)\.

Follow these three steps to perform full data load using mydumper\.

1. Produce a dump file containing source data\.

1. Restore this dump file on the target database using myloader\.

1. Retrieve the binary log position for ongoing replication\.

For example, the following command creates the backup of DbName1 and DbName2 databases using mydumper\.

```
mydumper \
--host=<db-server-address> \
--user=<mydumper-username> --password=<mydumper-password> \
--outputdir=/db-dump/mydumper-files/ \
-G -E -R --compress --build-empty-files \
--threads=4 --compress-protocol \
--regex '^(DbName1\.|DbName2\.)' \
-L /<mydumper-logs-dir>/mydumper-logs.txt
```

For example, the following command restores the backup to the Amazon RDS instance using myloader\.

```
myloader \
--host=<rds-instance-endpoint> \
--user=<db-username> --password=<db-password> \
--directory=<mydumper-output-dir> \
--queries-per-transaction=50000 --threads=4 \
--compress-protocol --verbose=3 -e 2><myload-output-logs-path>
```

For example, the following command retrieves the binary log information from the mydumper metadata file\.

```
cat <mydumper-output-dir>/metadata
# It should display data similar to the following:
SHOW MASTER STATUS:SHOW MASTER STATUS:
    Log: mysql-bin.000129
    Pos: 150
    GTID:
```

**Note**  
To ensure a valid dump file of logical backups in mysqldump and mydumper, don’t run data definition language \(DDL\) statements while the dump process is running\. It is recommended to schedule a maintenance window for these operations\. For details, see the [single\-transaction documentation](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html#option_mysqldump_single-transaction)\.
While exporting the data with logical backups, it is recommended to exclude MySQL default schemas \(mysql, performance\_schema, and information\_schema\), functions, stored procedures, and triggers\.
Remove definers from schema files before uploading extracted data to Amazon RDS\. For more information, see [How can I resolve definer errors](https://aws.amazon.com/premiumsupport/knowledge-center/definer-error-mysqldump)\.
Any backup operation acquires a global read lock on all tables \(using FLUSH TABLES WITH READ LOCK\)\. As soon as this lock has been acquired, the binary log coordinates are read and the lock is released\. For more information, see [Establishing a Backup Policy](https://dev.mysql.com/doc/mysql-backup-excerpt/5.7/en/backup-policy.html)\. For logical backups this step done at the beginning of the logical dump, however for physical backup \(Percona XtraBackup\) this step done at the end of backup\.