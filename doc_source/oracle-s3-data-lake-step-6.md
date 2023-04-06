# Step 6: Create an AWS DMS Task<a name="oracle-s3-data-lake-step-6"></a>

Before you create the replication task, it is important to understand the workload on the source database and usage pattern of the tables being replicated\. This helps plan an effective migration approach and minimize any configuration or workload related issues\. In this section, we first review the important considerations and then learn how to configure our walkthrough DMS task accordingly by applying table mappings and task settings\.

## Considerations Before Creating an AWS DMS Task<a name="oracle-s3-data-lake-step-6-considerations"></a>

 **Size and number of records** 

The volume of migrated records affects the full load completion time\. It is difficult to predict the full load time upfront, but testing with a replica of a production instance should provide a baseline\. Use this estimate to decide whether you should parallelize full load by using multiple tasks or by using the parallel load option\.

To speed up the full load of large tables such as sales table in our use case, we can increase the number of tables and partitions loaded in parallel up to 49\. The default value for the number of tables and partitions loaded in parallel is eight\. For more information about parallel load task settings, see [Full\-load task settings](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TaskSettings.FullLoad.html)\.

The `MaxFullLoadSubTasks` parameter controls number of tables or partitions loaded in parallel during full load\.

 **Transactions per second** 

While full load is affected by the number of records, the ongoing replication performance relies on the number of transactions on the source Oracle database\. Performance issues during change data capture \(CDC\) generally stem from resource constraints on the source database, replication instance, target database, and network bandwidth or throughput\. Knowing average and peak TPS on the source and recording CDC throughput and latency metrics helps baseline AWS DMS performance and identify an optimal task configuration\. For more information, see [Replication task metrics](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Monitoring.html#CHAP_Monitoring.Metrics.Task)\.

In this walkthrough, the source database is a data warehouse where transaction volume is not always high because the data is loaded on a periodic basis from the Online Transaction Processing \(OLTP\) layer\. Also, we run a heterogeneous data migration using the AWS DMS parallel load to migrate large tables with improved performance\. For more information, see [Using parallel load for selected tables, views, and collections](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Tablesettings.html#CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Tablesettings.ParallelLoad)\.

This approach requires a replication instance with higher compute capacity if the data volume is huge\. We chose the compute intensive c5 class replication instance to speed up the process\.

If you are not sure about your data volumes or performance expectations from the migration task, start with general t3 class instances, and then migrate to c5 class instances for compute intensive tasks or r5 class instances for memory intensive tasks\. You should monitor the task metrics continuously and choose the appropriate instance class that best suits your needs\.

 **Unsupported data types** 

Identify data types used in tables and check that AWS DMS supports these data types\. For more information, see [Source data types for Oracle](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html#CHAP_Source.Oracle.DataTypes)\.

Validate that the target Amazon S3 has the corresponding data types\. For more information, see [Target data types for S3 Parquet](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.S3.html#CHAP_Target.S3.DataTypes)\.

After you run the initial load test, validate that AWS DMS converted data as you expected\. You can also initiate a pre\-migration assessment to identify any unsupported data types in the migration scope\. For more information, see [Specifying individual assessments](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.AssessmentReport1.html#CHAP_Tasks.AssessmentReport1.Individual)\.

 **Source Database Workload** 

Running AWS DMS replication tasks for large tables can add to the workload on the source database especially during the full load phase when AWS DMS reads whole tables from source database without any filters to restrict rows\. When you use filters in AWS DMS task table mapping, confirm that appropriate indexes exist on the source tables and indexes are actually being used during full load\. Regularly monitor the source database to identify any workload related issues\. For more information, see [Using table mapping to specify task settings](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.html)\.

**Note**  
The previous list isn’t complete\. For more information, see [Best practices](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.html)\.

Combining the considerations from the previous list, we start with a single task that migrates all eight tables in parallel\. Based on the full load run time and resource utilization metrics on the source Oracle database instance and replication instance, we used AWS DMS parallel load option to further improve full load performance\.

## Task Configuration<a name="oracle-s3-data-lake-step-6-configuration"></a>

In this walkthrough, we migrate the incremental changes to the fact tables to the data lake\. To do so, we use the Full Load \+ CDC option\. For more information about the AWS DMS task creation steps and available configuration options, see [Creating a task](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.Creating.html)\.

We will first focus on the following settings\.

 **Table mappings** 

Use selection rules to define the schemas and tables that the AWS DMS task migrates\. For more information, see [Selection rules and actions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Selections.html)\.

In this walkthrough, we are migrating all the tables \(`%`\) in the sales history `SH` schema\. Another option is to include each table explicitly in the table mappings\. However, that increases operational overhead by requiring repeated configurations\. If we plan to add new tables to source database in future under the sales history schema, we should include all tables \(`%`\) in table mapping\.

**Note**  
Mapping rules are applied at the task level\. Make sure that you add a mapping rule to each task that replicates data to your data lake\. For our use case we need a single task\.

 **LOB settings** 

 AWS DMS handles large binary objects \(LOBs\) columns differently compared to other data types\. For more information, see [Migrating large binary objects \(LOBs\)](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.html#CHAP_BestPractices.LOBS)\.

A detailed explanation of LOB handling by AWS DMS is out of scope for this walkthrough\. However, remember that increasing the `LOB Max Size` increases the task’s memory utilization\. Because of that, we recommended that you don’t set `LOB Max Size` to a large value\. For more information about LOB settings, see [Task Configuration](https://docs.aws.amazon.com/dms/latest/sbs/chap-rdssqlserver2s3datalake.steps.createtask.html#chap-rdssqlserver2s3datalake.steps.createtask.configuration)\.

The source data warehouse schema in this walkthrough doesn’t include LOB data\. When you migrate LOB columns, make sure that you perform analysis on these columns\. Because AWS DMS doesn’t support Full LOB mode for Amazon S3 endpoints, we need to identify a suitable `LOB Max Size`\.

 **Parallel load** 

Though, we used significantly large instance class in previous run, overall improvement wasn’t significant because the data volume is relatively large\. The sales fact table includes 5 billion records\. To further optimize the performance, we used parallel\-load ranges option\. For more information, see [Using parallel load for selected tables, views, and collections](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Tablesettings.html#CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Tablesettings.ParallelLoad)\.

The following code example shows the mapping rule that we used\. As you can see, we defined 16 boundaries to cover data from 1998 to 2026 in 16 ranges\. With this option, full load finished in about 6\.5 hours\. As a result, we reduced the time taken to complete full load to almost one third as compared to initial load\.

```
{
    "rules": [
        {
            "rule-type": "selection",
            "rule-id": "653647496",
            "rule-name": "653647496",
            "object-locator": {
                "schema-name": "SH",
                "table-name": "SALES"
            },
            "rule-action": "include",
            "filters": []
        },
        {
            "rule-type": "table-settings",
            "rule-id": "653647497",
            "rule-name": "653647497",
            "object-locator": {
                "schema-name": "SH",
                "table-name": "SALES"
            },
            "parallel-load": {
                "type": "ranges",
                "columns": [
                    "TIME_ID"
                ],
                "boundaries": [
                    [
                        "1998-01-01 00:00:00"
                    ],
                    [
                        "2000-01-01 00:00:00"
                    ],
                    [
                        "2002-01-01 00:00:00"
                    ],
                    [
                        "2004-01-01 00:00:00"
                    ],
                    [
                        "2006-01-01 00:00:00"
                    ],
                    [
                        "2008-01-01 00:00:00"
                    ],
                    [
                        "2010-01-01 00:00:00"
                    ],
                    [
                        "2012-01-01 00:00:00"
                    ],
                    [
                        "2014-01-01 00:00:00"
                    ],
                    [
                        "2016-01-01 00:00:00"
                    ],
                    [
                        "2018-01-01 00:00:00"
                    ],
                    [
                        "2020-01-01 00:00:00"
                    ],
                    [
                        "2022-01-01 00:00:00"
                    ],
                    [
                        "2024-01-01 00:00:00"
                    ],
                    [
                        "2026-01-01 00:00:00"
                    ]
                ]
            }
        }
.
.
.
    ]
}
```

You can also use the `partitions-auto` option instead of ranges option because the `SALES` table is already partitioned\. In our testing, we found that with the ranges option, full load finishes faster\. So, we chose ranges option\.

 **Other task settings** 

Choose **Enable CloudWatch Logs** to upload the AWS DMS task run log to Amazon CloudWatch\. You can use these logs to troubleshoot issues because they include error and warning messages, start and end times of the run, configuration issues, and so on\. To diagnose performance issues, you can use changes to the task logging setting, such as enabling debug or trace\.

**Note**  
CloudWatch log usage is charged at standard rates\. For more information, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

For **Target table preparation mode**, choose one of the following options: **Do nothing**, **Truncate**, and **Drop**\. Use **Truncate** in data pipelines where the downstream systems rely on a fresh dump of clean data and do not rely on historical data\. In this walkthrough, we choose **Do nothing** because we want to control the retention of files from previous runs\.

For **Maximum number of tables to load in parallel**, enter the number of parallel threads that AWS DMS initiates during the full load\. You can increase this value to improve the full load performance and minimize the load time when you have numerous tables\. Because we have several partitions that AWS DMS can load in parallel, we used the maximum value of 49\.

**Note**  
Increasing this parameter induces additional load on the source database, replication instance, and target database\.

 **To create a database migration task** 

1. Sign in to the AWS Management Console, and open the [AWS DMS console](https://console.aws.amazon.com/dms/v2)\.

1. Choose **Database migration tasks**, then choose **Create task**\.

1. On the **Create database migration task** page, enter the following information\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/oracle-s3-data-lake-step-6.html)

1. Keep the default values for other parameters, and choose **Create task**\.

 AWS DMS runs the task immediately\. The **Database migration tasks** section displays the status of the migration task\.