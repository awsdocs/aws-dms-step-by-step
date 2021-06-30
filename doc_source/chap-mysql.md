# Migrating MySQL\-Compatible Databases to AWS<a name="chap-mysql"></a>

Amazon Web Services \(AWS\) has several services that allow you to run a MySQL\-compatible database on AWS\. Amazon Relational Database Service \(Amazon RDS\) supports MySQL\-compatible databases including MySQL, MariaDB, and Amazon Aurora MySQL\. AWS Elastic Cloud Computing Service \(EC2\) provides platforms for running MySQL\-compatible databases\.


| Migrating From | Solution | 
| --- | --- | 
|  An RDS MySQL DB instance  |  You can migrate data directly from an Amazon RDS MySQL DB snapshot to an Amazon Aurora MySQL DB cluster\. For details, see [Migrating Data from an Amazon RDS MySQL DB Instance to an Amazon Aurora MySQL DB Cluster](chap-mysql2aurora.rdsmysql.md)\.  | 
|  A MySQL database external to Amazon RDS  |  If your database supports the InnoDB or MyISAM tablespaces, you have these options for migrating your data to an Amazon Aurora MySQL DB cluster: \* You can create a dump of your data using the `mysqldump` utility, and then import that data into an existing Amazon Aurora MySQL DB cluster\. \* You can copy the source files from your database to an Amazon Simple Storage Service \(Amazon S3\) bucket, and then restore an Amazon Aurora MySQL DB cluster from those files\. This option can be considerably faster than migrating data using `mysqldump`\. For details, see [Migrating MySQL to Amazon Aurora MySQL by Using mysqldump](chap-mysql2aurora.md#chap-mysql2aurora.mysqldump)\.  | 
|  A database that is not MySQL\-compatible  |  You can also use AWS Database Migration Service \(AWS DMS\) to migrate data from a MySQL database\. However, for very large databases, you can significantly reduce the amount of time that it takes to migrate your data by copying the source files for your database and restoring those files to an Amazon Aurora MySQL DB instance as described in [Migrating Data from an External MySQL Database to an Amazon Aurora MySQL Using Amazon S3](chap-mysql2aurora.md#chap-mysql2aurora.s3)\. For more information on AWS DMS, see [What Is AWS Database Migration Service?](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html)   | 