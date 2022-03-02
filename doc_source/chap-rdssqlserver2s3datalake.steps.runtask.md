# Step 7: Run the AWS DMS Task<a name="chap-rdssqlserver2s3datalake.steps.runtask"></a>

After you created your AWS Database Migration Service \(AWS DMS\) task, run the task a few times to identify the full load run time and ongoing replication performance\. You can validate that initial configurations work as expected\. You can do this by monitoring and documenting resource utilization on the source database, replication instance, and target database\. These details make up the initial baseline and help determine if you need further optimizations\.

After you started the task, the full load operation starts loading tables\. You can see the table load completion status in the **Table Statistics** section and the corresponding target files in the Amazon S3 bucket\. Because in our case the overall number of records is less than 200,000, the full load operation finishes in less than a minute\. We can increase the value of **Maximum number of tables to load in parallel**, but it will not provide any meaningful gain in this scenario\.

After the AWS DMS task completes full load, the status changes to the **Load complete, replication ongoing** phase\. The following image shows the updated status of the task\.

![\[Load complete\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdssqlserver2s3datalake-load-complete-replication-ongoing.png)

During this phase, AWS DMS partitions data by the year, month, and day of generation\. The following image shows the structure of folders\.

![\[the structure of folders after partitioning.\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdssqlserver2s3datalake-partitioning.png)

Following, find some of the common errors and unexpected results you might see while following this walkthrough\.

 **Files are not written to the Amazon S3 target even though changes are visible in the table statistics section of the c console** 
+ This happens due to the target endpoint configuration\. After you set `CdcMaxBatchInterval=3600` and `CdcMinFileSize=64000`, AWS DMS waits for an hour or for the file size to reach 64 MB before writing data to Amazon S3\.
+ To write the output to Amazon S3 sooner, reduce `CdcMaxBatchInterval` to a smaller value\. Alternatively, you can stop and resume the task\. This will force Amazon S3 to flush events to Amazon S3 disregarding the extra connection attributes settings\. Using these options means that the size of CDC files will be much smaller than the expected 64 MB\.

 **Parquet file sizes are less than 64 MB despite setting `CdcMinFileSize=64000` ** 

 AWS DMS creates 64 MB files in memory\. When this data is encoded as Parquet the resulting file size is smaller\. The file sizes vary based on the level of compression possible\.

 ** AWS DMS captures only inserts and deletes and does not migrate update records to the target** 

You can see the following warning in the task logs:

```
00008570: 2021-12-07T19:52:52 [SOURCE_CAPTURE  ]W:  MS-REPLICATION is not enabled for table '[Sales].[SalesPerson]'. Therefore, UPDATE changes to it will not be captured. If you want UPDATE changes to be captured, either define a Primary Key for the table (if missing) or enable Microsoft CDC instead.  (sqlserver_log_utils.c:1292)
```

This log message indicates MS\-Replication\. However, for Amazon RDS for SQL Server you can use MS\-CDC\. This error occurs when you have not turned on MS\-CDC for the table\. For more information, see [Step 2: Configure a Source Amazon RDS for SQL Server Database](chap-rdssqlserver2s3datalake.steps.configuresource.md)\.

In this walkthrough, we covered most prerequisites that help avoid configuration related errors\. If you observe issues when running the task, see [Troubleshooting migration tasks in AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html), [Best practices for AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.html), or reach out to AWS Support for further assistance\.

After you completed the migration, validate that your data migrated successfully and delete the cloud resources that you created\.