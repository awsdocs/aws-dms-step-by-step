# Step 4: Store Database Credentials in AWS Secrets Manager<a name="schema-conversion-oracle-aurora-mysql-step-4"></a>

To connect to your source and target databases with DMS Schema Conversion, store your database credentials in AWS Secrets Manager\. Make sure that you replicate these secrets to your AWS Region\.

 **To store your source database credentials in AWS Secrets Manager ** 

1. Sign in to the AWS Management Console and open the AWS Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Choose your AWS Region\.

1. Choose **Store a new secret**\. The **Choose secret type** page opens\.

1. For **Secret type**, choose **Credentials for other database**\.

1. For **User name** and **Password**, enter the credentials of the database user that you created for your source database in [Step 2](schema-conversion-oracle-aurora-mysql-step-2.md)\.

1. For **Database**, choose **Oracle**\.

1. For **Server name**, **Database name**, and **Port**, enter your Oracle database connection information\.

1. Choose **Next**\. The **Configure secret** page opens\.

1. For **Secret name**, enter `sc-oracle-secret`\.

1. Choose **Next**\. The **Configure rotation** page opens\.

1. Choose **Next**\. The **Review** page opens\.

1. Choose **Store**\.

 **To store your target database credentials in AWS Secrets Manager ** 

1. Sign in to the AWS Management Console and open the AWS Secrets Manager console at [https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\.

1. Choose your AWS Region\.

1. Choose **Store a new secret**\. The **Choose secret type** page opens\.

1. For **Secret type**, choose **Credentials for Amazon RDS database**\.

1. For **User name** and **Password**, enter the credentials of the database user that you created for your target database in [Step 3](schema-conversion-oracle-aurora-mysql-step-3.md)\.

1. For **Database**, choose your Aurora MySQL DB instance\.

1. Choose **Next**\. The **Configure secret** page opens\.

1. For **Secret name**, enter `sc-mysql-secret`\.

1. Choose **Next**\. The **Configure rotation** page opens\.

1. Choose **Next**\. The **Review** page opens\.

1. Choose **Store**\.

Use these secrets when you create your migration project in [Step 7](schema-conversion-oracle-aurora-mysql-step-7.md)\.