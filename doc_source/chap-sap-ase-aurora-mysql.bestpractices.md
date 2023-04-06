# Best Practices<a name="chap-sap-ase-aurora-mysql.bestpractices"></a>
+ When the AWS Database Migration Service \(AWS DMS\) task completes the full load, AWS DMS stops this task\. You can take this opportunity to add or enable your foreign keys or constraints and triggers in your target\. If your migration type is **Migrate existing data and replicate ongoing changes**, resume the task to pick up the cached changes\.
+ When you have tasks running, you can monitor the on\-premises source host, your replication instance, and your target Amazon Aurora database\. Make sure that you create alarms and get notified on key metrics, such as central processing unit \(CPU\) utilization, freeable memory, and IOPS\.
+ Before you start the AWS DMS migration, make sure that you disables foreign keys and triggers on the target database\. Additionally, make sure that your user has privileges on AWS DMS and Amazon Relational Database Service \(Amazon RDS\)\.
+  AWS DMS recommends to periodically monitor the exception tables using the following query:

  ```
  select STATEMENT from admin."awsdms_apply_exceptions" where TASK_NAME in ('TASK NAME')
  ```
+ Monitoring the AWS DMS CloudWatch log for errors and warnings\. In case of any problem with migration, AWS DMS records a corresponding warning or error message in the log\.
+ Set up monitoring of source and target database latency to understand the replication lag\.
+ Use the AWS DMS data validation feature to detect data issues\.
+ Test the steps designed for migration to understand any unforeseen issues\.

This walkthrough covers proven end\-to\-end steps for migrating an SAP ASE database to Amazon Aurora MySQL using AWS DMS\. The walkthrough also includes basic instructions that show how to perform a similar migration\.