# Step 1: Install the SQL Drivers and AWS Schema Conversion Tool on Your Local Computer<a name="chap-sqlserver2aurora.steps.installsct"></a>

First, install the SQL drivers and the AWS Schema Conversion Tool \(AWS SCT\) on your local computer\. Do the following:

1. Download the JDBC driver for Microsoft SQL Server [mssql\-jdbc\-7\.2\.2\.jre11\.jar](https://docs.microsoft.com/en-us/sql/connect/jdbc/release-notes-for-the-jdbc-driver?view=sql-server-ver15#72)\.

1. Download the [JDBC driver for Aurora MySQL](https://dev.mysql.com/downloads/connector/j/)\. Amazon Aurora MySQL uses the MySQL driver\.

1. Install AWS SCT and the required JDBC drivers\.

   1. See [https://docs\.aws\.amazon\.com/SchemaConversionTool/latest/userguide/CHAP\_Installing\.html](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Installing.html), and choose the appropriate link to download AWS SCT\.

   1. Start AWS SCT, and choose **Settings**, **Global Settings**\.

   1. In **Global Settings**, choose **Drivers**, and then choose **Browse** for **Microsoft Sql Server Driver Path**\. Locate the JDBC driver for SQL Server, and choose **OK**\.

   1. Choose **Browse** for **MySql Driver Path**\. Locate the JDBC driver you downloaded for Aurora MySQL, and choose **OK**\.  
![\[Locating JDBC Drivers\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsqlserver2aurora-drivers.png)

   1. Choose **OK** to close the **Global Settings** dialog box\.