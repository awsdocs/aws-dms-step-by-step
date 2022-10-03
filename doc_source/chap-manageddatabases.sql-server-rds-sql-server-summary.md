# Summary<a name="chap-manageddatabases.sql-server-rds-sql-server-summary"></a>

The following table helps understand how each migration approach fits to different use cases\.


| SQL Server native tools | Data transformation | Table filtering | Metadata rename | Migration of secondary objects | Data validation | 
| --- | --- | --- | --- | --- | --- | 
|  Backup and restore  |  No  |  No  |  No  |  Yes  |  No  | 
|  Import and export wizard  |  Yes  |  Yes  |  Yes  |  No  |  Yes  | 
|  SQL Server \- Generate and Publish Scripts Wizard and bulk copy program utility \(bcp\)  |  No  |  Yes  |  No  |  No  |  No  | 

You can see that the SQL Server backup and restore has the best performance among the three full load options\. This is the preferred approach where the database size is less than 16 TB and when you don’t have transformation or filtering requirements\. Backup and restore has the additional advantage of migrating your secondary database objects such as stored procedures, functions, and so on\.

SQL Server Import and Export Wizard supports a wide range of features\. Consider this approach as the next option to evaluate if you don’t need to migrate secondary database objects such as views, stored procedures, triggers, and so on\. Also, use this approach to overcome the backup and restore limitations\. SQL Server Import and Export can also be used for smaller migrations where ease of use considerations override the minor performance gains provided by SQL Server Backup and Restore\.

Using Generate and Publish Scripts Wizard and bulk copy program utility \(bcp\) is slower than SQL Server Import and Export Wizard\. You can use this approach in some cases because in bcp you can parallelize the load\. That said, data files created by bcp may be orders of magnitude larger than the original table size\. Because of this, you might need a significant amount of storage space when you use bcp to migrate in parallel\.