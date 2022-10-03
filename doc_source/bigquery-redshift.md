# Migrating a BigQuery Project to Amazon Redshift<a name="bigquery-redshift"></a>

This walkthrough gets you started with heterogeneous database migration from BigQuery to Amazon Redshift\. To automate the migration, we use the AWS Schema Conversion Tool \(AWS SCT\) that runs on Windows\. This introductory exercise provides you with a good understanding of the steps involved in such a migration\.

At a high level, the steps involved in this migration are the following:
+ Use the Google Cloud management console to do the following:
  + Create a service account, which AWS SCT can use to connect to your source BigQuery project\.
  + Create a Cloud Storage bucket to store your source data during migration\.
+ Use the AWS Management Console to do the following:
  + Create an Amazon Redshift cluster\.
  + Create an Amazon Simple Storage Service \(Amazon S3\) bucket\.
+ Use AWS SCT to convert source database schemas and apply converted code to your target database\.
+ Use data extraction agents to migrate data\.

To see all the steps of the migration process, [watch this video](https://youtu.be/EdKB0tXFnoI)\.

This walkthrough takes approximately three hours to complete\. Make sure that you delete resources at the end of this walkthrough to avoid additional charges\.

**Topics**
+ [Prerequisites](bigquery-redshift-prerequisites.md)
+ [Migration Overview](bigquery-redshift-migration-overview.md)
+ [Step\-by\-Step Migration](bigquery-redshift-step-by-step-migration.md)
+ [Next Steps](bigquery-redshift-next-steps.md)