# Step 7: Create a Migration Project<a name="schema-conversion-sql-server-mysql-step-7"></a>

Now you can create a migration project which is the foundation of your work with DMS Schema Conversion\. A migration project describes your source and target data providers, your instance profile, and migration rules\.

To create a migration project

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose your AWS Region\.

1. Choose **Migration projects**, and then choose **Create migration project**\.

1. For **Name**, enter a unique name for your migration project\. For example, enter `sc-project`\.

1. For **Instance profile**, choose `sc-instance`\. You created this instance profile in [Step 5](schema-conversion-sql-server-mysql-step-5.md)\.

1. For **Source**, choose **Browse**, and then choose `sc-sql-server`\. You created this data provider in [Step 6](schema-conversion-sql-server-mysql-step-6.md)\.

1. For **Secret ID**, choose `sc-sql-server-secret`\. You created this secret in [Step 4](schema-conversion-sql-server-mysql-step-4.md)\.

1. For **IAM role**, choose `sc-secrets-manager-role`\. You created this role in [Step 1](schema-conversion-sql-server-mysql-step-1.md)\.

1. For **Target**, choose **Browse**, and then choose `sc-mysql`\. You created this data provider in [Step 6](schema-conversion-sql-server-mysql-step-6.md)\.

1. For **Secret ID**, choose `sc-mysql-secret`\. You created this secret in [Step 4](schema-conversion-sql-server-mysql-step-4.md)\.

1. For **IAM role**, choose `sc-secrets-manager-role`\. You created this role in [Step 1](schema-conversion-sql-server-mysql-step-1.md)\.

1. Choose **Create migration project**\.

Use this migration project to convert your SQL Server database schemas to MySQL\.