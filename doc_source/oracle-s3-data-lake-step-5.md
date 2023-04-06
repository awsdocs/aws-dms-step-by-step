# Step 5: Configure an AWS DMS Target Endpoint<a name="oracle-s3-data-lake-step-5"></a>

In this section, we walk through the configuration for setting up target data lake AWS DMS endpoint\. You will also select appropriate options to store files in data lake\.

To use Amazon S3 as an AWS Database Migration Service \(AWS DMS\) target endpoint, create an IAM role with write and delete access to the AWS DMS bucket\. Then add `dms.amazonaws.com` as a trusted entity in this IAM role\. For more information, see [Prerequisites for using Amazon S3 as a target](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Target.S3.html#CHAP_Target.S3.Prerequisites)\.

When you use AWS DMS to migrate data to an Amazon Simple Storage Service \(Amazon S3\) data lake, you can change the default task behavior, such as file formats, partitioning, file sizing, and so on\. This leads to minimizing post\-migration processing and helps downstream applications consume data efficiently\. You can customize task behavior using endpoint settings and extra connection attributes \(ECA\)\. Most of the AWS DMS endpoint settings and ECA settings overlap, except for a few parameters\. In this section of walkthrough, we configure AWS DMS endpoint settings\.

## Choose File Format<a name="oracle-s3-data-lake-step-5-file-format"></a>

For this walkthrough, we use the Parquet file format to help the data scientists consume data for data exploration and data discovery activities\. Apache Parquet is a columnar format, which is built to support efficient compression and encoding schemes providing storage space savings and performance benefits\.

Specify the following endpoint settings\.

```
DataFormat=parquet
ParquetVersion=PARQUET_2_0
```

## Determine File Size<a name="oracle-s3-data-lake-step-5-file-size"></a>

By default, during ongoing replication AWS DMS task write calls to Amazon S3 are triggered either if the file size reaches 32 KB or if the previous file write was more than 60 seconds ago\. These settings ensure that the data capture latency is less than a minute\. However, this approach creates numerous small files in target Amazon S3 bucket\.

Because we migrate our source Sales History database schema for a machine learning use case, some latency is acceptable\. However, we need to optimize this schema for cost and performance\.

During the data discovery phase performed by the data scientists, it is helpful to have large files for efficient analysis using the tools of their choice\. We recommend that you set the size of the target file to at least 64 MB\. Specify the following endpoint settings: `CdcMaxBatchInterval=3600` and `CdcMinFileSize=64000`\. These settings ensure that AWS DMS writes the file until its size reaches 64 MB or if the last file write was more than an hour ago\.

**Note**  
Parquet files created by AWS DMS are usually smaller than the specified `CdcMinFileSize` setting because Parquet data compression ratio varies depending on the source data set\. The size of CSV files created by AWS DMS is equal to the value specified in `CdcMinFileSize`\.

## Turn on S3 Partitioning<a name="oracle-s3-data-lake-step-5-partitioning"></a>

Partitioning in Amazon S3 structures your data by folders and subfolders that help efficiently query data\. For example, if you receive sales record data daily from different regions, and you query data for a specific region and find stats for a few months, then you can partition data by \{Product/source/region\}, year, and month\.

The following example shows the path In Amazon S3 for our use case\.

```
s3://<sales-anlaytics-bucket-name>/<Project/Source/Region>/<schemaname>/<tablename>/<year>-<month>-<day>

s3://s3-datalake
  - s3://s3-datalake/Oracledb
    - s3://s3-datalake/Oracledb/Sales
      - s3://s3-datalake/Oracledb/Sales/Products/
        - s3://s3-datalake/Oracledb/Sales/Products/LOAD00000001.parquet
      - s3://s3-datalake/Oracledb/Sales/Customer
        - s3://s3-datalake/Oracledb/Sales/Customer/LOAD00000001.parquet
          - s3://s3-datalake/Oracledb/Sales/Sales/Products/20222-10-23/
          - s3://s3-datalake/Oracledb/Sales/Sales/Products/2022-10-23/20221023-013830913.parquet
          - s3://s3-datalake/Oracledb/Sales/Sales/Products/2022-10-24/20221024-175902985.parquet
```

Partitioning provides performance benefits because data scanning will be limited to the amount of data in the specific partition based on the filter condition in your queries\. For our sales data example, data scientists' queries might look as follows:

```
SELECT <column-list> FROM <sales-hist-table-name> WHERE <region> = <region-name> AND <date> = <date-to-query>
```

When performing data exploration, the data scientists can consume the incremental load using partitions\. Partitioning the data helps read only latest data from the Amazon S3 bucket\. In this case, you explore the latest data and use it for training the models to determine latest sales trends\.

The following code example shows how to turn on partitioning for ongoing changes\.

```
bucketFolder=Oracledb
DatePartitionedEnabled=true
DatePartitionSequence=YYYYMMDD
DatePartitionDelimiter=DASH
```

**Note**  
The date partition delimiter is chosen as `DASH` because it creates prefixes in the format `YYYY-MM-DD` rather than `YYYY/MM/DD` format\. The advantage of using `DASH` is that it makes the 3 console view better with the files from each date \(`YYYY-MM-DD`\) being a single folder rather than having different folders for Year, month, and date\. This will also let users query for a particular date in a simpler manner\.

## Serialize Ongoing Replication Events<a name="oracle-s3-data-lake-step-5-ongoing-replication"></a>

A common challenge when using Amazon S3 as a target involves identifying the ongoing replication event sequence when multiple records are updated at the same time on the source database\.

 AWS DMS provides two options to help serialize such events for Amazon S3\. You can use the `TimeStampColumnName` endpoint setting or use transformation rules to include the Oracle System Change Number \(SCN\) column\. The `TimeStampColumnName` setting adds another `STRING` column to the target file created by AWS DMS\. During the ongoing replication, the column value represents the commit timestamp of the event in the Oracle database\. For the full load phase, the column values represent the timestamp of data transfer to Amazon S3\. The second option adds another column to include Oracle SCN\. You can use this field when the source database might have transactions that are occurring within a microsecond or if the source database doesn’t offer microsecond level precision\.

Because the sales history table doesn’t have a primary key column, we add the `Timestamp` column according to the option to add **TimeStampColumnName** which will serve as a unique identifier during data exploration and model training phases of machine learning\. We chose the option of timestamp over Oracle SCN because partitioning the data by timestamp will help data scientists for data exploration based on various criteria such as seasonal demand or product promotions\.

This setting is done at a task level\. Make sure that you repeat it for each task separately that migrates data from the Oracle database endpoint\.

For more information about this option, see [Step 6: Create an AWS DMS Task](oracle-s3-data-lake-step-6.md)\.

 **To create a target endpoint** 

1. Sign in to the AWS Management Console, and open the [AWS DMS console](https://console.aws.amazon.com/dms/v2)\.

1. Choose **Endpoints**, then choose **Create endpoint**\.

1. On the **Create endpoint** page, enter the following information\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/oracle-s3-data-lake-step-5.html)

1. Expand the **Endpoint settings** section, choose **Wizard**, and then choose **Add new setting** to add the following information\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/oracle-s3-data-lake-step-5.html)

1. Choose **Create endpoint**\.