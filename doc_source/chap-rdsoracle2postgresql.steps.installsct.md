# Step 1: Install the SQL Drivers and AWS Schema Conversion Tool on Your Local Computer<a name="chap-rdsoracle2postgresql.steps.installsct"></a>

To install the SQL drivers and the AWS Schema Conversion Tool \(AWS SCT\) on your local computer, do the following:

1. Download the JDBC driver for your Oracle database release\. For more information, go to [https://www\.oracle\.com/jdbc](https://www.oracle.com/jdbc)\.

1. Download the PostgreSQL driver \([postgresql\-42\.2\.19\.jar](https://jdbc.postgresql.org/download/postgresql-42.2.19.jar)\)\.

1. Install AWS SCT and the required JDBC drivers\.

   1. Download AWS SCT from [Installing, verifying, and updating the Schema Conversion Tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Installing.html)\.

   1. Launch AWS SCT\.

   1. In AWS SCT, choose **Global settings** from **Settings**\.

   1. In **Global settings**, choose **Driver**, and then choose **Browse** for **Oracle driver path**\. Locate the JDBC Oracle driver and choose **OK**\.

   1. Choose **Browse** for **PostgreSQL driver path**\. Locate the JDBC PostgreSQL driver and choose **OK**\.  
![\[Drivers\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sct-drivers.png)

   1. Choose **OK** to close the dialog box\.