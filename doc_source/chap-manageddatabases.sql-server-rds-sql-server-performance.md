# Performance Comparison<a name="chap-manageddatabases.sql-server-rds-sql-server-performance"></a>

To compare the full load migration performance for all three methods, we used a test environment\. In this environment, we populated the `dms_sample` database with 410\.90 GB of data\. We used the same on\-premise SQL Server source and RDS SQL Server target databases to load data three times\. For these data loads, we used the following methods:
+ Backup and restore\.
+ Import and export wizard\.
+ Generate and publish scripts wizard and bulk copy program utility \(bcp\)\.

The following image represents the performance comparison of the three migration methods\. We expect similar performance trends for larger datasets\.

![\[performance comparison of the three migration methods\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sql-server-rds-sql-server-performance.png)

The elapsed time shown in the diagram is the actual migration time\. It doesn’t include the time spent on implementing prerequisites\.

For the backup and restore method, we spent 4\.24 hours\. This time includes:
+ 1\.66 hours to backup the database\.
+ 1\.75 hours to copy the data from backup location to Amazon S3\.
+ 0\.88 hours to restore the data from the S3 bucket to Amazon RDS for SQL Server\.

For the import and export wizard, we spent 8\.58 hours\.

For the bcp method, we spent 199 hours\. This time includes:
+ 0\.01 hours to generate scripts\.
+ 0\.01 hours to run the generated script on Amazon RDS for SQL Server\.
+ 27\.88 hours to run the bcp statements for unloading data from on\-premise SQL Server\.
+ 171\.1 hours to run the bcp statements for loading data into Amazon RDS for SQL Server\.