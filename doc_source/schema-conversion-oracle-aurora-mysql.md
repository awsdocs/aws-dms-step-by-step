# Migrating Oracle databases to Amazon Aurora MySQL with DMS Schema Conversion<a name="schema-conversion-oracle-aurora-mysql"></a>

This walkthrough gets you started with heterogeneous database migration from Oracle to Amazon Aurora MySQL\-Compatible Edition\. To automate the migration, we use the AWS DMS Schema Conversion\. This service helps assess the complexity of your migration and converts source Oracle database schemas and code objects to a format compatible with MySQL\. Then, you apply the converted code to your target database\. This introductory exercise shows how you can use DMS Schema Conversion for this migration\.

At a high level, this migration includes the following steps:
+ Use the AWS Management Console to do the following:
  + Create a VPC in the Amazon VPC console\.
  + Create IAM roles in the IAM console\.
  + Create an Amazon S3 bucket in the Amazon S3 console\.
  + Create your target Aurora MySQL database in the Amazon RDS console\.
  + Store database credentials in AWS Secrets Manager\.
+ Use the AWS DMS console to do the following:
  + Create an instance profile for your migration project\.
  + Create data providers for your source and target databases\.
  + Create a migration project\.
+ Use DMS Schema Conversion to do the following:
  + Assess the migration complexity and review the migration action items\.
  + Convert your source database\.
  + Apply the converted code to your target database\.

This walkthrough takes approximately three hours to complete\. Make sure that you delete resources at the end of this walkthrough to avoid additional charges\.

**Topics**
+ [Prerequisites](schema-conversion-oracle-aurora-mysql-prerequisites.md)
+ [Migration Overview](schema-conversion-oracle-aurora-mysql-migration-overview.md)
+ [Step\-by\-Step Migration](schema-conversion-oracle-aurora-mysql-step-by-step-migration.md)
+ [Next Steps](schema-conversion-oracle-aurora-mysql-next-steps.md)