# Step 6: Create an AWS DMS Task<a name="chap-rdssqlserver2s3datalake.steps.createtask"></a>

After you configured the replication instance and endpoints, you need to analyze your source database\. A good understanding of the workload helps plan an effective migration approach and minimize configuration issues\. Find some of the important considerations following and learn how they apply to our walkthrough\.

 **Size and number of records** 

The volume of migrated records affects the full load completion time\. It is difficult to predict the full load time upfront, but testing with a replica of a production instance should provide a baseline\. Use this estimate to decide whether you should parallelize full load by using multiple tasks or by using the parallel load option\.

The `Sales` schema includes 19 tables\. The `CreditCard` table is the largest table containing 100,000 records\. We can increase the number of tables loaded in parallel to 19 if the full load is slow\. The default value for the number of tables loaded in parallel is eight\.

 **Transactions per second** 

While full load is affected by the number of records, the ongoing replication performance relies on the number of transactions on the source Amazon RDS for SQL Server database\. Performance issues during change data capture \(CDC\) generally stem from resource constraints on the source database, replication instance, target database, and network bandwidth or throughput\. Knowing average and peak TPS on the source and recording CDC throughput and latency metrics help baseline \(AWS DMS\) performance and identify an optimal task configuration\. For more information, see [Replication task metrics](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Monitoring.html#CHAP_Monitoring.Metrics.Task)\.

In this walkthrough, we will track the CDC latency and throughput values after the task moves into the ongoing replication phase to baseline AWS DMS performance\.

 **LOB columns** 

 AWS DMS handles large binary objects \(LOBs\) columns differently compared to other data types\. For more information, see [Migrating large binary objects \(LOBs\)](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.html#CHAP_BestPractices.LOBS)\.

Because AWS DMS does not support **Full LOB mode** for Amazon S3 endpoints, we need to identify a suitable **LOB Max Size**\.

A detailed explanation of LOB handling by AWS DMS is out of scope for this walkthrough\. However, remember that increasing the **LOB Max Size** increases the tasks memory utilization\. Because of that, it is recommended not to set **LOB Max Size** to a large value\.

For more information about LOB settings, see [Task Configuration](#chap-rdssqlserver2s3datalake.steps.createtask.configuration)\.

 **Unsupported data types** 

Identify data types used in tables and check that AWS DMS supports these data types\. For more information, see [Source data types for SQL Server](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SQLServer.html#CHAP_Source.SQLServer.DataTypes)\.

Validate that the target Amazon S3 has the corresponding data types\. For more information, see [Target data types for S3 Parquet](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.S3.html#CHAP_Target.S3.DataTypes)\.

After running the initial load test, validate that AWS DMS converted data as you expected\. You can also initiate a pre\-migration assessment to identify any unsupported data types in the migration scope\. For more information, see [Specifying individual assessments](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.AssessmentReport1.html#CHAP_Tasks.AssessmentReport1.Individual)\.

**Note**  
The preceding list is not complete\. For more information, see [Best practices for AWS Database Migration Service](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.html)\.

Combining the considerations from the preceding list, we start with a single task that migrates all 19 tables\. Based on the full load run time and resource utilization metrics on the source SQL Server database instance and replication instance, we can evaluate if we should parallelize the load further to improve performance\.

## Task Configuration<a name="chap-rdssqlserver2s3datalake.steps.createtask.configuration"></a>

In an AWS DMS task, you can specify the schema or table to migrate, the type of migration, and the configurations for the migration\. You can choose one of the following options for your task\.
+  **Full Load only** — migrate existing data\.
+  **Full Load \+ CDC** — migrate existing data and replicate ongoing changes\.
+  **CDC only** — replicate ongoing changes\.

For more information about the task creation steps and available configuration options, see [Creating a task](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.Creating.html)\.

In this walkthrough, we will focus on the following settings\.

 **Table mappings** 

Use selection rules to define the schemas and tables that the AWS DMS task will migrate\. For more information, see [Selection rules and actions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Selections.html)\.

Because we need to identify monthly sales for a specific year, one possible approach can restrict the migration to `SalesOrder%` tables in the `Sales` schema and keep adding new tables to the task when additional reporting is required\. This approach saves cost and minimizes the load, but increases operational overhead by requiring repeated configurations, performance baselining, and so on\. For the walkthrough, we will migrate all tables \(`%`\) from the `Sales` schema\.

 **Using transformation rules to include LSN column** 

In the previous section we discussed using the `TimestampColumnName` endpoint setting to serialize ongoing replication events\. For more information about using the `TimestampColumnName` endpoint setting, see [Serialize ongoing replication events](chap-rdssqlserver2s3datalake.steps.targetendpoint.md#chap-rdssqlserver2s3datalake.steps.targetendpoint.considerations.serialize)\.

Because the source database transaction log precision is limited to milliseconds, multiple events can have the same timestamp\. To address this issue, you can use task level transformation rules to include source table headers to the Amazon S3 target files as described in the task creation section\.

Source table headers add an additional column that contains the log sequence number \(LSN\) value of the operation from the source SQL Server database instance\. You can use this information in our Amazon S3 data lake scenario for downstream serialization\. For more information about source table headers, see [Replicating source table headers using expressions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Expressions.html#CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Expressions-Headers)\.

To include headers, add the following transformation rule in the JSON editor in table mapping\. This rule adds a new `transact-id` column with the LSN to all tables that the task migrates\. For more information, see [Specifying table selection and transformations rules using JSON](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.html)\.

```
{
    "rule-type": "transformation",
    "rule-id": "2",
    "rule-name": "2",
    "rule-target": "column",
    "object-locator": {
        "schema-name": "%",
        "table-name": "%"
    },
    "rule-action": "add-column",
    "value": "transact_id",
    "expression": "$AR_H_STREAM_POSITION",
    "data-type": {
        "type": "string",
        "length": 50
    }
}
```

**Note**  
Mapping rules are applied at the task level\. You need to add a mapping rule to each task that replicates data to your data lake\.

 **LOB settings** 

Use the **sys** schema to identify the LOB columns in the tables of the `Sales` schema\.

```
SELECT s.name AS SchemaName,
t.name AS TableName,
c.name AS ColumnName,
y.name AS DataType
FROM sys.tables AS t
INNER JOIN sys.schemas AS s ON s.schema_id = t.schema_id
INNER JOIN sys.columns AS c ON t.object_id = c.object_id
INNER JOIN sys.types AS y ON y.user_type_id = c.user_type_id
WHERE (c.user_type_id in (34,35,99,129,130,241,256) OR (c.user_type_id in (165,167,231) AND c.max_length = -1))
AND s.name = 'Sales'
ORDER BY t.name;
```

The Sales\.Store table includes one LOB column\. Use the following query to identify the size of the largest LOB in the migrated tables\.

```
select max(datalength(Demographics)) as "Size in Bytes" from Sales.Store
```

The size of the largest LOB is 1,000 bytes\. Because of that, we will leave the default value for `LOB Max Size`, which is 32 KB\. If the size of the largest LOB is more than 32 KB, it is recommended to factor in LOB growth over time, include some buffer, and set that as the `LOB Max Size` value\.

 **Other task settings** 

Choose **Enable CloudWatch Logs** to upload the AWS DMS task execution log to AWS CloudWatch\. You can use these logs to troubleshoot issues because they include error and warning messages, start and end times of the run, configuration issues, and so on\. Changes to the task logging setting, such as enabling debug or trace can also be helpful to diagnose performance issues\.

**Note**  
CloudWatch log usage is charged at standard rates\. For more information, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

For **Target table preparation mode**, choose one of the following options: `Do nothing`, `truncate`, and `Drop`\. Use `Truncate` in data pipelines where the downstream systems rely on a fresh dump of clean data and do not rely on historical data\. In this walkthrough, we choose **Do nothing** because we want to control the retention of files from previous runs\.

For **Maximum number of tables to load in parallel**, enter the number of parallel threads that AWS DMS initiates during full load\. You can increase this value to improve the full load performance and minimize the load time when you have numerous tables\.

**Note**  
Increasing this parameter induces additional load on the source database, replication instance, and target database\.

## Create an AWS DMS Task<a name="chap-rdssqlserver2s3datalake.steps.createtask.create"></a>

After you completed all settings configurations, you can create an AWS DMS database migration task\.

To create a database migration task, do the following:

1. Open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose **Database migration tasks**, and then choose **Create task**\.

1. On the **Create database migration task** page, enter the following information\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdssqlserver2s3datalake.steps.createtask.html)

1. Leave the default values in the other fields and choose **Create task**\.

1. The task begins immediately\. The **Database migration tasks** section shows you the status of the migration task\.