# Migrating a SQL Server Database to Amazon Aurora MySQL<a name="chap-sqlserver2aurora"></a>

Using this walkthrough, you can learn how to migrate a Microsoft SQL Server database to an Amazon Aurora MySQL\-Compatible Edition database using the AWS Schema Conversion Tool \(AWS SCT\) and AWS Database Migration Service \(AWS DMS\)\. AWS DMS migrates your data from your SQL Server source into your Aurora MySQL target\.

 AWS DMS doesn’t migrate your secondary indexes, sequences, default values, stored procedures, triggers, synonyms, views, and other schema objects that aren’t specifically related to data migration\. To migrate these objects to your Aurora MySQL target, use AWS SCT\.

To estimate what it will cost to run this walkthrough on AWS, you can use the AWS Pricing Calculator\. For more information, see [https://calculator\.aws/](https://calculator.aws/)\.

To avoid additional charges, delete all resources after you complete the walkthrough\.

**Topics**
+ [Prerequisites](chap-sqlserver2aurora.prerequisites.md)
+ [Step\-by\-Step Migration](chap-sqlserver2aurora.steps.md)
+ [Troubleshooting](chap-sqlserver2aurora.steps.troubleshooting.md)