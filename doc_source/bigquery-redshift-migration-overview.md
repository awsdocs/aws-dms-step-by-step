# Migration Overview<a name="bigquery-redshift-migration-overview"></a>

This section provides high\-level guidance for customers looking for a way to migrate from BigQuery to Amazon Redshift\. After you complete this introductory exercise, understand the migration process, and become familiar with migration automation tools, plan the migration of your production workloads\.

The following illustration demonstrates the migration architecture for this walkthrough\.

![\[Migrate a snapshot into Amazon Aurora MySQL\]](http://docs.aws.amazon.com/dms/latest/sbs/images/bigquery-redshift-migration-architecture.png)

First, you create a service account to connect to your BigQuery project\. Then you create an Amazon Redshift database, as well as the buckets in Cloud Storage and Amazon S3\. After this setup, you use AWS SCT to convert source database schemas and apply them to your target database\. Finally, you install and configure a data extraction agent to migrate data, upload it to your S3 bucket, and then copy to Amazon Redshift\. For big datasets, you can use several data extraction agents to increase the speed of data migration\.

To connect to BigQuery, AWS SCT uses the Application Layer Transport Security \(ALTS\) authentication and encryption system\. To connect to Amazon S3 and Amazon Redshift, AWS SCT uses the HTTPS and SSL protocols\.

## Migration Strategy<a name="bigquery-redshift-migration-overview-strategy"></a>

For BigQuery to Amazon Redshift migrations, you can use the following typical migration approach\.

1.  **Future State Architecture Design** 

   This step defines the architecture of your new system in the target environment\. This architecture includes databases, applications, scripts, and so on\.

1.  **Database Schema Conversion** 

   You can use AWS SCT to automate the conversion of your source database to Amazon Redshift\. For more information, see [Convert Database Schemas](bigquery-redshift-migration-step-6.md)\.

1.  **Application Conversion or Remediation** 

   After you migrate your data storage, make sure that you update your applications\. You can use AWS SCT to convert SQL queries in your application code\. For more information, see [Converting SQL code in your applications](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.App.Generic.html)\.

1.  **Scripts, ETL, Reports Conversion** 

   In addition to applications, make sure that you update all other components of your source system\. These include business intelligence reports, extract, transform, and load \(ETL\) processes, and other scripts\.

1.  **Integration with Third\-Party Applications** 

   Your applications usually connect to other applications or monitoring tools\. Your migration from BigQuery to Amazon Redshift affects these dependencies\.

1.  **Data Migration** 

   You can use AWS SCT to manage a data extraction agent that migrates data from BigQuery to Amazon Redshift\. For more information, see [Data Extraction Agents](bigquery-redshift-migration-step-7.md)\.

1.  **Testing and Bug Fixing** 

   Migration touches all the stored procedures and functions and affects substantial parts of the application code\. For this reason, good testing is required both at the unit and system functional level\.

1.  **Performance Tuning** 

   Because of database platform differences and syntax, certain constructs or combinations of data objects can perform differently on the new platform\. The performance tuning part of the migration resolves any bottlenecks\.

1.  **Setup, DevOps, Integration, Deployment, and Security** 

   Take the opportunity to embrace infrastructure as code for the migration\. Make sure that you also focus on the application security\. Finally, plan the cutover\.

## Security in the AWS Cloud<a name="bigquery-redshift-migration-overview-security"></a>

Cloud security at AWS is the highest priority\. As an AWS customer, you benefit from a data center and network architecture that is built to meet the requirements of the most security\-sensitive organizations\.

 AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. For more information, see [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)\.

Amazon Redshift protects data with AWS encryption solutions, along with all default security controls within AWS services\. Your data is encrypted at rest and in transit\. Amazon Redshift automatically integrates with AWS Key Management Service \(AWS KMS\) for key management\. AWS KMS uses envelope encryption\. For more information, see [Data protection in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/security-data-protection.html)\.

Access to Amazon Redshift requires credentials that AWS can use to authenticate your requests\. Those credentials must have permissions to access AWS resources, such as an Amazon Redshift cluster\. You can use AWS Identity and Access Management \(IAM\) to secure your data by controlling who can access your Amazon Redshift cluster\. For more information, see [Identity and access management in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html)\.

## Data Types Mapping<a name="bigquery-redshift-migration-overview-mapping"></a>

Amazon Redshift supports all BigQuery data types\. The following table shows the data type mappings that AWS SCT uses by default\. Users can set up migration rules in AWS SCT to change the data type of columns\. For more information, see [Creating migration rules](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.html#CHAP_Converting.MigrationRules)\.


| BigQuery data type | Amazon Redshift data type | 
| --- | --- | 
|  BOOLEAN  |  BOOLEAN  | 
|  BYTES\(L\)  |  BINARY VARYING\(L\)  | 
|  BYTES  |  BINARY VARYING\(1024000\)  | 
|  DATE  |  DATE  | 
|  DATETIME  |  TIMESTAMP WITHOUT TIME ZONE  | 
|  GEOGRAPHY  |  GEOGRAPHY  | 
|  INTERVAL  |  CHARACTER VARYING\(256\)  | 
|  JSON  |  SUPER  | 
|  INTEGER  |  BIGINT  | 
|  NUMERIC\(p,s\)  |  NUMERIC\(p,s\)  | 
|  NUMERIC  |  NUMERIC\(38,9\)  | 
|  BIGNUMERIC  |  NUMERIC\(38,9\)  | 
|  BIGNUMERIC\(p,s\)  |  NUMERIC\(p,s\) if *p* is less than or equal to 38 or *s* is less than or equal to 37\.  | 
|  BIGNUMERIC\(p,s\)  |  CHARACTER VARYING\(256\) if *p* is more than 38 or *s* is more than 37\.  | 
|  FLOAT  |  DOUBLE PRECISION  | 
|  STRING\(L\)  |  CHARACTER VARYING\(L\) if *L* is less than 65,535\.  | 
|  STRING  |  CHARACTER VARYING\(65535\)  | 
|  STRUCT  |  SUPER  | 
|  TIME  |  TIME WITHOUT TIME ZONE  | 
|  TIMESTAMP  |  TIMESTAMP WITHOUT TIME ZONE  | 

## Limitations<a name="bigquery-redshift-migration-overview-limitations"></a>

You can use AWS SCT to automatically convert a majority of your BigQuery code and storage objects\. These objects include datasets, tables, views, stored procedures, functions, data types, and so on\. However, AWS SCT has some limitations when using BigQuery as a source\.

For example, AWS SCT can’t convert subqueries in analytic functions, as well as geography, statistical aggregate, or some of the string functions\. You can find the full list of limitations in the AWS SCT user guide\. For more information, see [Limitations on using BigQuery as a source](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Source.BigQuery.html#CHAP_Source.BigQuery.Limitations)\.