# SQL Server Import and Export Wizard<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-import-export"></a>

Microsoft SQL Server Import and Export Wizard is a high\-performance option for data migration\. It uses the SQL Server Integration Services \(SSIS\) framework\. For more information, see [Import and Export Data with the SQL Server Import and Export Wizard](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard?view=sql-server-ver15) and [SQL Server Integration Services](https://docs.microsoft.com/en-us/sql/integration-services/sql-server-integration-services?view=sql-server-ver15)\.

The Import and Export Wizard is suitable for the following use cases:
+ To achieve high migration performance\.
+ To transform data during the migration\. You can use the wizard to create SSIS packages and modify them in Visual Studio with an SSIS extension to achieve this\.
+ To rename the target tables or schemas during the migration\.
+ To migrate only the tables and avoid the migration of the secondary database objects such as users, views, stored procedures, triggers, foreign keys or functions\.

The migration performance is affected by resource constraints of the host where you run the wizard\. During the migration, all data is funneled through this host\.

## Migration Steps<a name="chap-manageddatabases.sql-server-rds-sql-server-full-load-import-export-steps"></a>

Use the following steps to migrate all the tables and views from the `dms_sample` database to your target database\.

Disable all constraints on the target DB instance before to the migration\. The Import and Export Wizard copies tables in a random order\. This may lead to failures if you enforce referential integrity on the target\.

```
EXEC sp_msforeachtable 'ALTER TABLE ? NOCHECK CONSTRAINT all'
```

Make sure that you capture the current log sequence number \(LSN\) from the source database before your start the full load\. To capture the current LSN, use the following command\.

```
SELECT max([Current LSN]) FROM fn_dblog(NULL, NULL)
```

Then you can use this LSN to set up the change data capture \(CDC\) task in AWS DMS\.

Open the SQL Server Import and Export Wizard from the Windows Start menu\. Connect to your source and target databases and select the source tables and views\. The following image shows the SQL Server Import and Export Wizard application window\.

![\[SQL Server Import and Export Wizard application window\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sql-server-rds-sql-server-full-load-import-export.png)

Choose **Next**, then choose **Run immediately**, and then choose **Finish**\. The SQL Server Import and Export Wizard starts the migration\. You can monitor the progress of your migration using the Performing Operation screen\. For more information, see [Performing Operation \(SQL Server Import and Export Wizard\)](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/performing-operation-sql-server-import-and-export-wizard?view=sql-server-ver15)\.

Make sure that you turn on constraints after you complete the migration\.

```
EXEC sp_msforeachtable 'ALTER TABLE ? CHECK CONSTRAINT all'
```

For more information, see [Get started with this simple example of the Import and Export Wizard](https://docs.microsoft.com/en-us/sql/integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard?view=sql-server-ver15)\.