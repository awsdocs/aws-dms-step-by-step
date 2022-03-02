# Performance Comparison<a name="chap-manageddatabases.mysql2rds.performance"></a>

We tested these three full load options using a Mysql 5\.7 database on EC2 as the source and Aurora MySQL 5\.7 as the target\. The source database contained the AWS DMS [sample database](https://github.com/aws-samples/aws-database-migration-samples/tree/master/mysql/sampledb/v1) with a total of 9 GB of data\. The following image shows the performance results\.

![\[Performance comparison of mysqldump\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-mysql2rds-performance-comparison.png)

Percona XtraBackup performed 4x faster than mysqldump and 2x faster than mydumper backups\. We tested larger datasets, for example with a total of 400 GB of data, and found that the performance scaled proportionally to the dataset size\.

Percona XtraBackup creates a physical backup of the database files whereas the other tools create logical backups\. Percona XtraBackup is the best option for full load if your use case conforms to the restrictions listed in the Percona XtraBackup section above\. If Percona XtraBackup isn’t compatible with your use case, mydumper is the next best option\. For more information about physical and logical backups, see [Backup and Recovery Types](https://dev.mysql.com/doc/refman/8.0/en/backup-types.html)\.