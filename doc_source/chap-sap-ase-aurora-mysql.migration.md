# Database Migration<a name="chap-sap-ase-aurora-mysql.migration"></a>

This section covers two major database migration tasks: code conversion and data load\. You can use AWS Schema Conversion Tool \(AWS SCT\) to convert database schema objects such as tables, views, procedures, functions, and so on\. Then you can use AWS Database Migration Service \(AWS DMS\) to load data\.

## Database Schema Conversion<a name="chap-sap-ase-aurora-mysql.migration.schema"></a>

To convert your database schema and code objects from SAP ASE to Amazon Aurora MySQL, follow these steps\.

1. Download and install AWS Schema Conversion Tool \(AWS SCT\) with the required SAP ASW and MySQL JDBC drivers\. For more information, see [https://docs\.aws\.amazon\.com/SchemaConversionTool/latest/userguide/CHAP\_Installing\.html](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Installing.html)\.

1. Create a new AWS SCT project, add your source and target databases, and add a mapping rule\. For more information, see [Creating a new project](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_UserInterface.html#CHAP_UserInterface.Project), [Adding database servers](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_UserInterface.html#CHAP_UserInterface.AddServers), and [Creating mapping rules](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Mapping.html)\.

1. Convert your database schema\. For more information, see [Converting database schemas](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.html)\.

1. Save the converted SQL scripts\. For more information, see [Saving and applying your converted schema](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.html#CHAP_Converting.SaveAndApply)\.

1. Run these scripts against your target MySQL database\. First, create tables with primary keys only\. Then add the foreign keys and secondary indexes after you complete the full load\.

## Migrate an SAP ASE Database to Amazon Aurora MySQL Using AWS DMS<a name="chap-sap-ase-aurora-mysql.migration.data"></a>

This section covers the steps that you follow to migrate an SAP ASE database to Amazon Aurora MySQL using AWS DMS\.

 AWS DMS creates the schema in the target if the schema doesn’t exist\. However, AWS DMS only creates the tables with primary keys\. AWS DMS doesn’t create foreign keys or secondary indexes\. Even the default values may be missing\.

The best practice is to create the schema objects using the scripts that AWS SCT generated in the prior step, then start AWS DMS to load table data\.

### Create a Replication Instance<a name="chap-sap-ase-aurora-mysql.migration.data.instance"></a>

To start data migration, create an AWS DMS replication instance\. For performance reasons, AWS DMS recommends creating the replication instance in the same AWS Region as your target Amazon Aurora database\. For more information, see [Creating a replication instance](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_ReplicationInstance.Creating.html)\.

### Create a Source Endpoint<a name="chap-sap-ase-aurora-mysql.migration.data.sourceendpoint"></a>

Create a source endpoint for SAP ASE and test the connection using the preceding replication instance\.
+ On the AWS DMS console, choose **Endpoints**\.
+ Choose **Create endpoint**\.
+ For **Endpoint type**, select **Source endpoint**\.
+ Enter your desired endpoint configuration\.  
![\[Source endpoint configuration\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sap-ase-to-aurora-mysql-endpoint-configuration.png)

  You can use your own on\-premises name server and a hostname instead of the IP address\. For more information, see [Using your own on\-premises name server](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.html#CHAP_BestPractices.Rte53DNSResolver)\.
+ Select the endpoint that you created, and choose **Test connection** from the **Actions** drop\-down menu\.

To use Transport Layer Security \(TLS\) for an SAP ASE database version 15\.7 and higher, use the Adaptive Server Enterprise 16\.03\.06 extra connection attribute \(ECA\) provider\. Use the following example:

```
provider=Adaptive Server Enterprise 16.03.06;
```

Make sure that you open the database port to the IP range of your replication instance before you test the connection\. If the firewall is open but you still experience a connection issue, please contact AWS support\.

### Create a Target Endpoint<a name="chap-sap-ase-aurora-mysql.migration.data.targetendpoint"></a>

Create a target endpoint for your Amazon Aurora MySQL database\.
+ On the AWS DMS console, choose **Endpoints**\.
+ Choose **Create endpoint**\.
+ For **Endpoint type**, select **Target endpoint**\.
+ Enter your desired endpoint configuration\.  
![\[Target endpoint configuration\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sap-ase-to-aurora-mysql-target-endpoint-configuration.png)
+ Test the connection using the preceding replication instance\.

To establish the connection, make sure that you edit the security group for your Amazon Aurora DB instance\. Also, open the 3306 port on your MySQL database to the private IP or IP range of the replication instance\.
+ On the Amazon Relational Database Service \(Amazon RDS\) console, choose your Amazon Aurora MySQL DB instance\.
+ On the **Connectivity & security** tab, locate your security group name under **Security**\.
+ Choose the security group link\. A new security group interface page opens\.
+ Choose **Inbound rules**\.
+ Choose **Edit inbound rules**\.
+ Add the IP range of the replication instance\.

### Create a Migration Task<a name="chap-sap-ase-aurora-mysql.migration.data.task"></a>

Create a migration task using the source and target endpoints that you created on the preceding step\. For more information, see [Creating a task](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.Creating.html)\. After you create your task, AWS DMS sets its status to **Ready**\. When you start or resume the task, AWS DMS changes the status to **Starting** or **Running**\.

To monitor the process, choose **Task Monitoring**, **Table Statistics**, **Logs**\. For more information, see [Monitoring Database Migration Service metrics](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Monitoring.html#CHAP_Monitoring.Metrics) and [How can I enable monitoring for an database migration task?](https://aws.amazon.com/premiumsupport/knowledge-center/dms-monitor-task/)\.

### Cutover Procedures<a name="chap-sap-ase-aurora-mysql.migration.data.cutover"></a>

When the AWS DMS task finishes the full load and applies cached changes, the task moves to the change data capture \(CDC\) stage\. At this point, you can perform the cutover to Amazon Aurora\. You run SQL queries to validate data and use AWS services to set up backup and monitor jobs\.

To perform the cutover, do the following:
+ Analyze the database queries in Amazon Aurora MySQL and test the performance of critical queries\.
+ Shut down all the application servers and stop all the client connections to SAP ASE\. Close any user sessions if necessary\.
+ Verify that the target data has been synced with the source database\.
+ Stop the AWS DMS task\.
+ Create the foreign keys and secondary indexes in Amazon Aurora MySQL if you didn’t create them before the CDC stage started\.
+ Validate tables, views, procedures, functions, and triggers within your schema\.
+ Switch the application servers, clients, and jobs to the Amazon Aurora MySQL database\.
+ Create the CloudWatch alarms based on your desired DB metrics\. For more information, see [Key Metrics for Amazon Aurora](https://aws.amazon.com/blogs/apn/key-metrics-for-amazon-aurora/) and [Monitoring an Amazon Aurora DB Cluster](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/MonitoringAurora.html)\.
+ Add a reader node to an existing Amazon Aurora MySQL cluster\. By default, Amazon Aurora replicates data across three Availability Zones in one Region at the storage level\. This architecture is fault tolerant by design\. For enhanced availability, add a reader node for a production database to automate failover in case of instance failure\. Modify the database cluster to enable failover in case of instance failure\. For more information, see [High Availability for Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Concepts.AuroraHighAvailability.html)\.

## Troubleshooting<a name="chap-sap-ase-aurora-mysql.migration.troubleshooting"></a>

For more information about troubleshooting issues with AWS DMS, see [Troubleshooting migration tasks in Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html)\.

For more information about troubleshooting issues specific to using AWS DMS with SAP ASE databases, see [Troubleshooting issues with SAP ASE](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.SAP)\.

For more information about troubleshooting issues specific to using AWS DMS with Amazon Aurora MySQL databases, see [Troubleshooting issues with MySQL](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.MySQL) and [Troubleshooting issues with Amazon Aurora MySQL](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Aurora)\.