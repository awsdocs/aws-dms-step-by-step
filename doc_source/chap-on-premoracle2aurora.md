# Migrating an On\-Premises Oracle Database to Amazon Aurora MySQL<a name="chap-on-premoracle2aurora"></a>

Following, you can find a high\-level outline and also a complete step\-by\-step walkthrough that both show the process for migrating an on\-premises Oracle database \(the source endpoint\) to an Amazon Aurora MySQL\-Compatible Edition \(the target endpoint\) using AWS Database Migration Service \(AWS DMS\) and the AWS Schema Conversion Tool \(AWS SCT\)\.

 AWS DMS migrates your data from your Oracle source into your Aurora MySQL target\. AWS DMS also captures data manipulation language \(DML\) and data definition language \(DDL\) changes that happen on your source database and apply these changes to your target database\. This way, AWS DMS helps keep your source and target databases in synch with each other\. To facilitate the data migration, DMS creates tables and primary key indexes on the target database if necessary\.

However, AWS DMS doesn’t migrate your secondary indexes, sequences, default values, stored procedures, triggers, synonyms, views and other schema objects not specifically related to data migration\. To migrate these objects to your Aurora MySQL target, use the AWS Schema Conversion Tool\.

We highly recommend that you follow along using the Amazon sample database\. To find a tutorial that uses the sample database and instructions on how to get a copy of the sample database, see [Working with the Sample Database for Migration](chap-on-premoracle2aurora.appendix.sampledatabase.md)\.

If you’ve used AWS DMS before or you prefer clicking a mouse to reading, you probably want to work with the high\-level outline\. If you need the details and want a more measured approach \(or run into questions\), you probably want the step\-by\-step guide\.


| Topic: Migration from On\-Premises Oracle to Aurora MySQL or Amazon RDS for MySQL | 
| --- | 
|   **Time:**   | 
|   **Cost:**   | 
|   **Source Database:** Oracle  | 
|   **Target Database:** Amazon Aurora MySQL/MySQL  | 
|   **Restrictions:**   **Oracle Edition:** Enterprise, Standard, Express and Personal  **Oracle Version:** 10g \(10\.2 and later\), 11g, 12c, \(On Amazon Relational Database Service \(Amazon RDS\), 11g or higher is required\.\)  **MySQL or Related Database Version:** 5\.5, 5\.6, 5\.7, MariaDB, Amazon Aurora MySQL  | 

## Costs<a name="chap-on-premoracle2aurora.costs"></a>

Because AWS DMS isn’t incorporated into the calculator yet, see the following table for a pricing estimate\.

In addition to the setup on your own PC, you must create several AWS components to complete the migration process\. The AWS components include:


|  AWS service  | Type | Description | 
| --- | --- | --- | 
|  Amazon Aurora MySQL DB instance  |  db\.r3\.large  |  Single AZ, 10 GB storage, 1 million I/O  | 
|   AWS DMS replication instance  |  T2\.large  |  50 GB of storage for keeping replication logs included  | 
|   AWS DMS data transfer  |  Free, based on the amount of data transferred for the sample database\.  |  | 
|  Data transfer out  |  First 1 GB per month free  |  | 