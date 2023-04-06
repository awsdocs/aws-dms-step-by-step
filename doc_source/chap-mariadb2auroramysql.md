# Migrating a MariaDB Database to Amazon RDS for MySQL or Amazon Aurora MySQL<a name="chap-mariadb2auroramysql"></a>

You can migrate data from existing on\-premises MariaDB or [Amazon RDS for MariaDB](https://aws.amazon.com/rds/mariadb/?nc=sn&loc=3&dn=4) to [Amazon Aurora](https://aws.amazon.com/rds/aurora/) MySQL using [Database Migration Service](https://aws.amazon.com/dms/)\. Amazon Aurora is a MySQL and PostgreSQL\-compatible relational database built for the cloud\. Amazon Aurora features a distributed, fault\-tolerant, self\-healing storage system that auto\-scales up to 64 TB per database instance\. It delivers high performance and availability with up to 15 low\-latency read replicas, point\-in\-time recovery, and continuous backup to Amazon S3, and replication across three Availability Zones \(AZs\)\.

Some key features offered by Aurora MySQL are the following:
+ High throughput with low latency
+ Push\-button compute scaling
+ Storage autoscaling
+ Custom database endpoints
+ Parallel queries for faster analytics

In the following sections, we demonstrate migration from MariaDB as a source database to an Aurora MySQL database as a target using AWS DMS\. At a high level, the steps involved in this migration are:
+ Provision MariaDB as a source DB instance and load the data
+ Provision Aurora Mysql as target DB instance
+ Provision DMS replication instance and create DMS endpoints
+ Create DMS task, migrate data and perform validation

For the purpose of this section, we are using the AWS CloudFormation templates for creating Amazon RDS for MariaDB, Aurora MySQL database and AWS DMS replication instance with their source and endpoints\. We will be loading sample tables and data in MariaDB located on [GitHub](https://github.com/aws-samples/aws-database-migration-samples)\.

To estimate what it will cost to run this walkthrough on AWS, you can use the AWS Pricing Calculator\. For more information, see [https://calculator\.aws/](https://calculator.aws/)\.

**Topics**
+ [Set up MariaDB as a source database](chap-mariadb2auroramysql.provisioningmariadb.md)
+ [Set up Aurora MySQL as a target database](chap-mariadb2auroramysql.provisioningauroramysql.md)
+ [Set up an AWS DMS replication instance](chap-mariadb2auroramysql.provisioningdms.md)
+ [Test the endpoints](chap-mariadb2auroramysql.testendpoints.md)
+ [Create a migration task](chap-mariadb2auroramysql.createtask.md)
+ [Validate the migration](chap-mariadb2auroramysql.validate.md)
+ [Cut over](chap-mariadb2auroramysql.cutover.md)