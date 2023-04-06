# Step 6: Configure Data Providers<a name="schema-conversion-oracle-postgresql-step-6"></a>

In this step, you create data providers that describe your source and target databases\. A data provider stores a data store type and the location information about your database\. Data providers don’t include database credentials\. You store database credentials in AWS Secrets Manager\. Make sure that you include data providers and database secrets in your DMS Schema Conversion migration project\.

You can create only one data provider for a single database\. If you try to create a second data provider for the same database, DMS Schema Conversion displays an error message\. However, you can use one data provider in multiple migration projects\.

 **To create a data provider for your Oracle database** 

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose your AWS Region\.

1. In the navigation pane, choose **Data providers**, and then choose **Create data provider**\.

1. For **Configuration**, choose **Enter manually**\.

1. For **Name**, enter a unique name for your source data provider\. For example, enter `sc-oracle`\.

1. For **Engine type**, choose **Oracle**\.

1. For **Server name**, enter the Domain Name Service \(DNS\) name or IP address of your database server\.

1. For **Port**, enter the port used to connect to your database server\.

1. For **Service ID \(SID\) or service name**, enter the Oracle System ID \(SID\)\. To find the Oracle SID, submit the following query to your Oracle database:

   ```
   SELECT sys_context('userenv','instance_name') AS SID FROM dual;
   ```

1. For **Secure Socket Layer \(SSL\) mode**, choose **none**\. Optionally, choose the type of your SSL enforcement, and provide the certificate information\.

1. Choose **Create data provider**\.

 **To create a data provider for your Amazon RDS for PostgreSQL database** 

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose your AWS Region\.

1. In the navigation pane, choose **Data providers**, and then choose **Create data provider**\.

1. For **Configuration**, choose **RDS database instance**\.

1. For **Database from RDS**, choose your Amazon RDS for PostgreSQL database\.

1. For **Name**, enter a unique name for your target data provider\. For example, enter `sc-postgresql`\.

1. Choose **Create data provider**\.

Use these data providers when you create your migration project in [Step 7](schema-conversion-oracle-postgresql-step-7.md)\.