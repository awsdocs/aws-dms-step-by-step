# Step 2: Install the SQL Tools and AWS Schema Conversion Tool on Your Local Computer<a name="chap-rdsoracle2redshift.steps.installsct"></a>

Next, you need to install a SQL client and AWS SCT on your local computer\.

This walkthrough assumes you will use the SQL Workbench/J client to connect to the RDS instances for migration validation\.

1. Download SQL Workbench/J from [the SQL Workbench/J website](http://www.sql-workbench.net/downloads.html), and then install it on your local computer\. This SQL client is free, open\-source, and DBMS\-independent\.

1. Download the JDBC driver for your Oracle database release\. For more information, go to [https://www\.oracle\.com/jdbc](https://www.oracle.com/jdbc)\.

1. Download the Amazon Redshift driver file, `RedshiftJDBC41-1.1.17.1017.jar`, as described following\.

   1. Find the Amazon S3 URL to the file in [Previous JDBC Driver Versions](https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc-previous-versions.html) of the *Amazon Redshift Cluster Management Guide*\.

   1. Download the driver as described in [Download the Amazon Redshift JDBC Driver](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html#download-jdbc-driver) of the same guide\.

1. Using SQL Workbench/J, configure JDBC drivers for Oracle and Amazon Redshift to set up connectivity, as described following\.

   1. In SQL Workbench/J, choose **File**, then choose **Manage Drivers**\.

   1. From the list of drivers, choose **Oracle**\.

   1. Choose the **Open** icon, then choose the `ojdbc.jar` file that you downloaded in the previous step\. Choose **OK**\.  
![\[AWS Database Migration Service driver management\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift7.png)

   1. From the list of drivers, choose **Redshift**\.

   1. Choose the **Open** icon, then choose the Amazon Redshift JDBC driver that you downloaded in the previous step\. Choose **OK**\.  
![\[AWS Database Migration Service driver management\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift8.png)

Next, install AWS SCT and the required JDBC drivers\.

1. Download AWS SCT from [Installing and Updating the AWS Schema Conversion Tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_SchemaConversionTool.Installing.html) in the *AWS Schema Conversion Tool User Guide\.* 

1. Follow the instructions to install AWS SCT\. By default, the tool is installed in the `C:\Program Files\AWS Schema Conversion Tool\AWS` directory\.

1. Launch AWS SCT\.

1. In AWS SCT, choose **Global Settings** from **Settings**\.

1. Choose **Settings**, **Global Settings**, then choose **Drivers**, and then choose **Browse** for **Oracle Driver Path**\. Locate the Oracle JDBC driver and choose **OK**\.

1. Choose **Browse** for **Amazon Redshift Driver Path**\. Locate the Amazon Redshift JDBC driver and choose **OK**\. Choose **OK** to close the dialog box\.  
![\[Connecting to the Oracle DB instance for AWS Database Migration Service\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift8.5.png)