# Migrating an Oracle Database to PostgreSQL<a name="chap-rdsoracle2postgresql"></a>

Using this walkthrough, you can learn how to migrate an Oracle database to a PostgreSQL database using AWS Database Migration Service \(AWS DMS\) and the AWS Schema Conversion Tool \(AWS SCT\)\.

 AWS DMS migrates your data from your Oracle source into your PostgreSQL target\. AWS DMS also captures data manipulation language \(DML\) and [supported data definition language \(DDL\)](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Introduction.SupportedDDL.html) changes that happen on your source database and applies these changes to your target database\. This way, AWS DMS keeps your source and target databases in sync with each other\. To facilitate the data migration, AWS SCT creates the migrated schemas on the target database, including the tables and primary key indexes on the target if necessary\.

 AWS DMS doesn’t migrate your secondary indexes, sequences, default values, stored procedures, triggers, synonyms, views, and other schema objects not specifically related to data migration\. To migrate these objects to your PostgreSQL target, use AWS SCT\.

**Topics**
+ [Prerequisites](chap-rdsoracle2postgresql.prerequisites.md)
+ [Step\-by\-Step Migration](chap-rdsoracle2postgresql.steps.md)
+ [Rolling Back the Migration](chap-oracle2postgresql.rollback.md)
+ [Troubleshooting](chap-oracle2postgresql.troubleshooting.md)