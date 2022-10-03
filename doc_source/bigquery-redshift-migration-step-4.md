# Step 4: Install AWS SCT on Your Local Computer<a name="bigquery-redshift-migration-step-4"></a>

In this step, you install and configure the AWS Schema Conversion Tool\. In this walkthrough, we run AWS SCT and the data extraction agent on Windows\. However, you can use AWS SCT and data extraction agents on other supported operating systems\. For more information, see [Installing the schema conversion tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Installing.html#CHAP_Installing.Procedure) and [Installing extraction agents](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/agents.dw.html#agents.Installing)\.

 **To install AWS SCT ** 

1. Download the compressed file that contains AWS SCT installer for Microsoft Windows from [https://s3\.amazonaws\.com/publicsctdownload/Windows/aws\-schema\-conversion\-tool\-1\.0\.latest\.zip](https://s3.amazonaws.com/publicsctdownload/Windows/aws-schema-conversion-tool-1.0.latest.zip)\.

1. Extract AWS SCT installer file\.

1. Run AWS SCT installer file that you extracted in the previous step\.

1. Choose **Next**, accept the terms of the License Agreement, and choose **Next** again\.

1. Enter the path to the folder where you want to install AWS SCT, and choose **Next**\.

1. Choose **Install**\.

1. Choose **Finish** to close the installation wizard\.

Now you can run AWS SCT\. Before you create a new project, make sure that you add the path to an Amazon Redshift JDBC driver in the application settings\. You don’t need a JDBC driver to connect to your BigQuery project\.

 **To configure driver settings in AWS SCT ** 

1. Download an Amazon Redshift JDBC driver version 2\.1\.0\.9 or later from [https://docs\.aws\.amazon\.com/redshift/latest/mgmt/jdbc20\-download\-driver\.html](https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-download-driver.html)\.

1. Extract the JDBC driver from the compressed file that you downloaded\.

1. Open AWS SCT, and choose **Global settings** from **Settings**\.

1. Choose **Drivers**\.

1. For **Amazon Redshift driver path**, choose Browse and choose the `redshift-jdbc42-2.1.0.9.jar` file that you extracted\.

1. Choose **Apply**, and then choose **OK** to close the settings window\.

To access AWS services such as Amazon S3 from AWS SCT, you configure an AWS service profile\. An AWS service profile is a set of AWS credentials that includes your AWS access key, AWS secret access key, AWS Region, and Amazon S3 bucket\.

 **To create an AWS service profile in AWS SCT ** 

1. Open AWS SCT, and choose **Global settings** from **Settings**\.

1. Choose ** AWS service profiles**\.

1. Choose **Add a new AWS service profile**\.

1. For **Profile name**, enter a descriptive name for your profile\.

1. For ** AWS access key**, enter your AWS access key\.

1. For ** AWS secret key**, enter your AWS secret access key\. For more information about AWS access keys, see [Programmatic access](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys)\.

1. For **Region**, choose the AWS Region where you created your Amazon S3 bucket in the previous step\.

1. For **Amazon S3 bucket folder**, choose the Amazon S3 bucket that you created in the previous step\.

1. Choose **Apply**, and then choose **OK** to close the settings window\.