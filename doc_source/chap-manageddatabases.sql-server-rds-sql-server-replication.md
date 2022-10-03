# Ongoing Replication<a name="chap-manageddatabases.sql-server-rds-sql-server-replication"></a>

After you complete the full load, set up ongoing replication using AWS DMS to keep the source and target databases synchronized\. To configure the ongoing replication task, open the AWS DMS console\. On the **Create database migration task** page, follow these three steps\.
+ For **Migration type**, select **Replicate ongoing changes**\.
+ Under **CDC start mode for source transactions**, select **Specify a log sequence number**\.
+ Under **System change number**, enter the SQL Server log sequence number that you captured during the full load\.

For more information, see [Continuous replication tasks](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Task.CDC.html)\.