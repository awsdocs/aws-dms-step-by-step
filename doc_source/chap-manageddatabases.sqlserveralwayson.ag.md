# SQL Server Always On Availability Groups<a name="chap-manageddatabases.sqlserveralwayson.ag"></a>

Always On availability groups provide high availability, disaster recovery, and read\-scale balancing\. These availability groups require a cluster manager\. The Always On availability groups feature provides an enterprise\-level alternative to database mirroring\. Introduced in SQL Server 2012 \(11\.x\), Always On availability groups maximizes the availability of a set of user databases for an enterprise\. An availability group supports a fail\-over environment for a discrete set of user databases, known as availability databases, that fail over together\. An availability group supports a set of read\-write primary databases and sets of corresponding secondary databases\. Optionally, secondary databases can be made available for read\-only access and/or some backup operations\.

## AWS DMS Use Case<a name="chap-manageddatabases.sqlserveralwayson.ag.usecase"></a>

A customer used AWS DMS to migrate data from a SQL Server 2017 source database\. This database was clustered in a 4\-node Always On Availability Group \(AAG\) configuration\. The customer configured the AWS DMS source endpoint to connect directly to the IP address of the primary node of the AAG by using an IP address\. With this setup, the customer used the AAG HA/DR functionality for internal applications\. In this case, AWS DMS can’t use the secondary database if a failover happens\. The customer used the target endpoint to populate an Operational Data Store \(ODS\) of the Amazon RDS for SQL Server database and an Amazon Simple Storage Service \(Amazon S3\) data lake\.

The following diagram displays the customer’s existing architecture\.

![\[Existing architecture\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-aws-dms-direct-ip-connection.png)

## Issues with This Approach<a name="chap-manageddatabases.sqlserveralwayson.ag.issues"></a>

Maintenance activities \(operating system patching, RDBMS patching\) can cause a server failover and AWS DMS will not be able to connect to the source\.

Activity and transactions continue to occur on the failover database as shown in the preceding image\. Because of this, the change data capture task becomes out of sync when the cluster fails back to the primary node\.

At the start of the task, AWS DMS polls all the nodes in Always On cluster for transaction backups\. The AWS DMS task can also fail if transaction backup happens from any other node than the primary\.

## The Solution Recommended by AWS DMS<a name="chap-manageddatabases.sqlserveralwayson.ag.solutions"></a>

To address connectivity design deficiencies, AWS DMS recommended to configure the AWS DMS source endpoint to connect to the AAG listener IP address or a canonical name record instead of connecting directly to the IP address of the primary node\. In case of a failover, AWS DMS will interact with the secondary databases, like any other application\. Without using the AAG listener IP address, AWS DMS will not be aware of the secondary replica to connect in case of a failover\.

The following diagram displays the proposed architecture\.

![\[Listener IP connection\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-aws-dms-listener-ip-connection.png)

 AWS DMS recommended to set the extra connection attribute `MultiSubnetFailover=True` in the customer’s AWS DMS endpoint\. This ODBC driver attribute helps AWS DMS connect to the new primary in case of an Availability Group failover\. This attribute is designed for situations when the connection is broken\. In these situations, AWS DMS attempts to connect to all IP addresses associated with the AAG listener\. For more information, see [Multi\-subnet failovers](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/listeners-client-connectivity-application-failover?view=sql-server-2017#SupportAgMultiSubnetFailover)\.

Also, AWS DMS recommended to set the extra connection attribute `alwaysOnSharedSynchedBackupIsEnabled=false` to poll all the nodes in Always On cluster for transaction backups\.

For more information on extra connection attributes for SQL Server as source, see [Extra connection attributes when using SQL Server as a source for AWS DMS](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html#CHAP_Source.SQLServer.ConnectionAttrib)\.