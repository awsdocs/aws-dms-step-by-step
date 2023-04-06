# Step 6: Configure Data Providers<a name="schema-conversion-sql-server-aurora-postgresql-step-6"></a>

In this step, you create data providers that describe your source and target databases\. A data provider stores a data store type and the location information about your database\. Data providers don’t include database credentials\. You store database credentials in AWS Secrets Manager\. Make sure that you include data providers and database secrets in your DMS Schema Conversion migration project\.

You can create only one data provider for a single database\. If you try to create a second data provider for the same database, DMS Schema Conversion displays an error message\. However, you can use one data provider in multiple migration projects\.

 **To create a data provider for your SQL Server database** 

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose your AWS Region\.

1. In the navigation pane, choose **Data providers**, and then choose **Create data provider**\.

1. For **Configuration**, choose **Enter manually**\.

1. For **Name**, enter a unique name for your source data provider\. For example, enter `sc-sql-server`\.

1. For **Engine type**, choose **Microsoft SQL Server**\.

1. For **Server name**, enter the Domain Name Service \(DNS\) name or IP address of your database server\.

1. For **Port**, enter the port used to connect to your database server\.

1. For **Database name**, enter the name of your database\.

1. For **Secure Socket Layer \(SSL\) mode**, choose **none**\. Optionally, choose the type of your SSL enforcement, and provide the certificate information\.

1. Choose **Create data provider**\.

 **To create a data provider for your Aurora PostgreSQL database** 

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose your AWS Region\.

1. In the navigation pane, choose **Data providers**, and then choose **Create data provider**\.

1. For **Configuration**, choose **RDS database instance**\.

1. For **Database from RDS**, choose your Aurora PostgreSQL database\.

1. For **Name**, enter a unique name for your target data provider\. For example, enter `sc-postgresql`\.

1. For **Database name**, enter the name of your database\.

1. For **Existing CA certificate**, choose the server certificate\. If you don’t have any server certificates, then for **Import certificate file**, provide the `rds-ca-2019.pem` file with your certificate\.

1. Choose **Create data provider**\.

Use these data providers when you create your migration project in [Step 7](schema-conversion-sql-server-aurora-postgresql-step-7.md)\.