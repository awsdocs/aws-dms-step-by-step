# Generate and Publish Scripts Wizard and Bulk Copy Program Utility<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-bcp"></a>

You can use the SQL Server Generate and Publish Scripts wizard to create Transact\-SQL scripts for objects in your database\. Then you can run the Bulk Copy Program Utility \(bcp\) to copy data from your Microsoft SQL Server instance into data files\. Also, you can use bcp to import data into a table from data files\. For more information, see [How to: Generate a Script \(SQL Server Management Studio\)](https://docs.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms178078(v=sql.105)?redirectedfrom=MSDN) and [bcp Utility](https://docs.microsoft.com/en-us/sql/tools/bcp-utility?view=sql-server-ver15)\.

This approach is suitable for the following use cases:
+ You don’t transform data during the migration\.
+ You don’t rename tables or schemas during the migration\.
+ You use referential integrity on target tables\. In this case, bcp automatically suspends RI constraints on target tables during data import\.
+ You can script and migrate all database objects using the SQL Server Generate and Publish Scripts wizard\.

This approach has the following limitations:
+ You need to create schemas on your target database before you can use migration scripts\.
+ This approach is slower than the Import and Export Wizard\.
+ Data transformations aren’t supported\.
+ In bcp, the error messages are limited to 512 bytes\. This can make troubleshooting complicated\.
+ You run the bcp command for each table\. This increases the complexity for large migrations\.

## Migration Steps<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-bcp-steps"></a>

At a high level, the steps involved in this approach are the following:
+ Use Microsoft Generate and Publish Scripts wizard to create Transact\-SQL scripts from source database\.
+ Use the created Transact\-SQL scripts to create database objects in Target database\.
+ Use SQL Server Bulk Copy Program Utility \(bcp\) to export data from the source database to data files\. Then, use bcp to import data from the data files into the target database table\.

The following example shows how to migrate the `dms_sample` database using Generate and Publish Scripts wizard and Bulk Copy Program Utility\.

Generate a Transact\-SQL script for the source database tables\. You can save the script as single file or save in a new query window\.

![\[Microsoft Generate and Publish Scripts wizard\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sql-server-rds-sql-server-full-load-bcp.png)

Next, create database objects on the target database using the script that you generated in the previous step\.

Run the following command on the source database to capture the current log sequence number \(LSN\)\. Then use this LSN to set up the change data capture \(CDC\) task in AWS DMS\.

```
SELECT max([Current LSN]) FROM fn_dblog(NULL, NULL)
```

Use the Windows command prompt to export the source tables to data files with the bcp utility\.

```
bcp [database_name.] schema.table_name out "data_file" -c -t -S [server_name] -d [database_name] -U [login] -P [password]
```

Import the data files created in the previous step into the target database with the bcp utility\.

```
bcp [database_name.] schema.table_name in "data_file" -S [server_name] -d [database_name] -c -t
```

You can create a \.bat file with all the bcp scripts to avoid running script one by one\. The following code example shows the contents of this \.bat file\.

```
bcp dbo.export1 out C:\BCP\export1.dat -c -t -S source-server-name -d dms_sample -U dms_user -P password
bcp dbo.export2 out C:\BCP\export2.dat -c -t -S source-server-name -d dms_sample -U dms_user -P password
bcp dbo.export3 out C:\BCP\export3.dat -c -t -S source-server-name -d dms_sample -U dms_user -P password
bcp dbo.export1 in C:\BCP\export1.dat -c -t -S target-server-name -d dms_sample -U dms_user -P password
bcp dbo.export2 in C:\BCP\export2.dat -c -t -S target-server-name -d dms_sample -U dms_user -P password
bcp dbo.export3 in C:\BCP\export3.dat -c -t -S target-server-name -d dms_sample -U dms_user -P password
```