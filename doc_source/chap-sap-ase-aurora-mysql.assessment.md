# Preparation and Assessment<a name="chap-sap-ase-aurora-mysql.assessment"></a>

Preparation and assessment of your source database is the initial phase\. Before you start moving data, you should monitor and analyze the source database schema for the data lifecycle\. To provide the best migration solution, you should have a good understanding on the workload, data access patterns, and data dependencies\. Make sure to consider the following items:
+ Character set\.
+ Largest table size\.
+ Largest LOB size\.
+ Integration with other databases or OS\.

## Determine the Character Set<a name="chap-sap-ase-aurora-mysql.assessment.characterset"></a>

To find out the default character set and sort order for your SAP ASE database, run the following query:

```
exec sp_default_charset
```

If your application uses a different character set, you can find it from your session by checking the global variable `@@client_csname` or `@@client_csid`\.

If you have a non\-default character set that you want to migrate, use an extra connect attribute to specify the character set for the source database\. For example, if your default character set is UTF8, specify `charset=utf8` as an extra connect attribute to correctly migrate data\.

## Determine the Largest Table Size<a name="chap-sap-ase-aurora-mysql.assessment.tablesize"></a>

Explore your largest and busiest tables to find out their size and rate of change\. This gives you an accurate estimate of where time will be spent when you do the initial data migration using the AWS Database Migration Service \(AWS DMS\) full load feature\.

You can parallelize the load on the table level with one task to save time by using parallel load feature\. For more information, see [Using parallel load for selected tables, views, and collections](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Tablesettings.html#CHAP_Tasks.CustomizingTasks.TableMapping.SelectionTransformation.Tablesettings.ParallelLoad)\.

In SAP ASE, you can run only one replication thread for each database\. Because of that, you can start one AWS DMS task at one time for each database\. You can’t run multiple tasks, which is common when you migrate from other database engines\. For more information, see [Limitations on using SAP ASE as a source](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.SAP.html#CHAP_Source.SAP.Limitations)\.

For SAP ASE version 15 and later, query `sysobjects` to list the top 10 in row count and space used\. Use the following query:

```
select top 10 convert(varchar(30),o.name) AS table_name,
    row_count(db_id(), o.id) AS row_count,
    data_pages(db_id(), o.id, 0) AS pages,
    data_pages(db_id(), o.id, 0) * (@@maxpagesize/1024) AS kbs
    from sysobjects o
    where type = 'U'
    order by kbs DESC, table_name ASC
```

The output of this query is shown following\.

```
table_name |row_count|pages|kbs|
salesdetail|      116|    2|  8|
```

## Determine the Largest LOB Size<a name="chap-sap-ase-aurora-mysql.assessment.lobsize"></a>

Large objects \(LOBs\) typically take the longest to migrate, unlike data types such as number and character, because of time spent encoding, storing, decoding, and retrieving them\. You should identify tables with the TEXT, UNITEXT, and IMAGE data types, because AWS DMS converts these objects to LOB\.

 AWS DMS recommends to identify the size of LOB columns and choose limited or full mode, and max LOB size in the LOB settings appropriately\. You can use the following dynamic SQL to generate a query for each table\.

```
select 'select max(datalength('+ c.name +'))/1024 KB_SIZE from dbo.'+ o.name+';'
from sysobjects o,
    syscolumns c
where o.type = 'U' and
    o.id = c.id and
    c.type in (34,35,174);
```

The output of this query is shown following\.

```
select max(datalength(pic))/1024 KB_SIZE from dbo.au_pix;
select max(datalength(copy))/1024 KB_SIZE from dbo.blurbs;
```

After you run the preceding queries, compare the results and choose the top one\.

For example, we found the largest LOB column in our SAP ASE database is 51 KB\. We used this number as input in our task settings with limited LOB mode\.

The speed of the full load is improved with the limited LOB mode compared to the full LOB mode\. For performance reasons, AWS DMS recommends to use limited LOB mode and increase the maximum LOB size to cover the actual size you find from your query\. For more information, see [How can I improve the speed of a migration task that has LOB data?](https://aws.amazon.com/premiumsupport/knowledge-center/dms-improve-speed-lob-data/)\.

## Document Integrations with Other Databases or Applications<a name="chap-sap-ase-aurora-mysql.assessment.integrations"></a>

Replace remote objects or interfaces in your code with other AWS services or equivalent external services\.

For example, in the SAP ASE database, you may send out email using the `xp_sendmail` procedure\. Because Amazon Aurora MySQL doesn’t provide native support for sending emails, redesign the process\. For example, use an AWS Lambda function to send email from the database\. For more information, see [AWS Lambda](http://aws.amazon.com/lambda) and [Sending notifications from Amazon Aurora MySQL](https://docs.amazonaws.cn/en_us/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.Lambda.html)\.

**Note**  
For database links from the source database to a remote server in SAP ASE, update data using foreign data wrappers \(FDW\)\.