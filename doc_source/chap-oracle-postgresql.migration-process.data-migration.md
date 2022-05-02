# Data Migration Mechanism<a name="chap-oracle-postgresql.migration-process.data-migration"></a>

For testing purposes and for production cutover, data needs to be migrated from the old Amazon RDS for Oracle instance to the new Amazon RDS or Aurora PostgreSQL instance\. Such a data migration requires knowledge of data type mapping and possibly incremental loading, depending on the size of the data and migration window\.

For this purpose AWS Database Migration Service \(AWS DMS\) can be used to connect source and target databases to replicate the contents of the data in the most optimal way\.

## Process<a name="chap-oracle-postgresql.migration-process.data-migration.process"></a>

1. Create a replication server\.

1. Create source and target endpoints that have connection information about your data stores\.

1. Create one or more migration tasks to migrate data between the source and target data stores\.

After you configured AWS DMS, you can perform the following operations:
+ A full data migration from Oracle to PostgreSQL\.
+ An ongoing replication from Oracle to PostgreSQL\.

Depending on the type of data in the database, you may need to optimize AWS DMS for handling certain data types like LOBS which you can read more about in the product guidance\.

## Reverse Migration<a name="chap-oracle-postgresql.migration-process.data-migration.reverse-migration"></a>

Normally you just fall back to the old system if a migration fails during smoke testing, and in most cases you may decide to fix forward after cutover, in which case you fix any unforeseen bugs in the migrated system\. But in some cases you may decide to have the option of migrating production data back from the new system to the original system after having been in production for a time\. In those cases, a reverse data migration mechanism must be configured\.

![\[Reverse Migration\]](http://docs.aws.amazon.com/dms/latest/sbs/images/oracle-postgresql-reverse-migration.png)

For more information, see [What is Database Migration Service?](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) and [Migrating Oracle databases with near\-zero downtime](https://aws.amazon.com/blogs/database/migrating-oracle-databases-with-near-zero-downtime-using-aws-dms/)\.