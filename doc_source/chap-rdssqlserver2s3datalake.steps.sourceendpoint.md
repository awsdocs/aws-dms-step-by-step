# Step 3: Create an AWS DMS Source Endpoint<a name="chap-rdssqlserver2s3datalake.steps.sourceendpoint"></a>

After you configured the AWS Database Migration Service \(AWS DMS\) replication instance and the source Amazon RDS for SQL Server instance, ensure connectivity between these two instances\. To ensure that the replication instance can access the server and the port for the database, make changes to the relevant security groups and network access control lists\. For more information about your network configuration, see [Setting up a network for a replication instance](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_ReplicationInstance.VPC.html)\.

After you completed the network configurations, you can create a source endpoint\.

To create a source endpoint, do the following:

1. Open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose **Endpoints**\.

1. Choose **Create endpoint**\.

1. On the **Create endpoint** page, enter the following information\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdssqlserver2s3datalake.steps.sourceendpoint.html)

1. Choose **Create endpoint**\.

**Note**  
To migrate a Microsoft SQL Server Always On database, you need to use different configurations\. For more information, see [Migrating a SQL Server Always On Database to AWS](chap-manageddatabases.sqlserveralwayson.md)\.