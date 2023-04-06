# Migrating an Amazon RDS for Oracle Database to an Amazon S3 Data Lake<a name="oracle-s3-data-lake"></a>

This walkthrough helps you understand the process of migrating data from Amazon Relational Database Service \(Amazon RDS\) for Oracle to Amazon Simple Storage Service \(Amazon S3\) using AWS Database Migration Service \(AWS DMS\)\.

Most organizations use Online Transaction Processing \(OLTP\) database engines to host their transactional workloads\. These engines are optimized for high\-transaction volumes such as an online order processing application\. However, these engines typically perform poorly for analytical applications, such as business intelligence or building predictive models using machine learning\. For these use cases, a popular solution is to build a data lake for analysis\.

In this document, we build a data lake in Amazon S3 using data hosted in an RDS for Oracle database\. Amazon S3 is the largest and most performant cloud storage service\. With Amazon S3, you can build a cost\-effective, secure data lake\. Amazon S3 provides 99\.999999999% \(11 9s\) of data durability and makes it possible to store and manage both structured and unstructured data at unlimited scale\.

To illustrate the process, we use AWS DMS to migrate data from an example database\. AWS DMS is a managed service that helps migrate between heterogeneous sources and targets\. In our case, we migrate an Oracle database to Amazon S3\. AWS DMS support not only the migration of your existing data, but also ensures that the source and target are synchronized for ongoing transactions\.

**Topics**
+ [Why use AWS DMS?](#oracle-s3-data-lake-why-dms)
+ [Example data set](#oracle-s3-data-lake-data-set)
+ [Solution Overview](oracle-s3-data-lake-solution-overview.md)
+ [Prerequisites](oracle-s3-data-lake-prerequisites.md)
+ [Step\-by\-Step Migration](oracle-s3-data-lake-step-by-step.md)
+ [Conclusion](oracle-s3-data-lake-conclusion.md)

## Why use AWS DMS?<a name="oracle-s3-data-lake-why-dms"></a>

You can use a SQL\-level mechanism to source ongoing changes\. This approach impacts your source database performance or requires that you implement additional logic\. For example, you can use SQL filters on last updated timestamps or add triggers to capture DML changes\. In contrast, AWS DMS mines changes from the database transaction logs, which are generated by the database for recovery purposes\. AWS DMS then takes those changes, converts them into the target format, and applies them to the target\. This process minimizes overhead on the source and provides near\-real time replication to the target\.

In the rest of this document, we guide you through the steps that you take to migrate the example Oracle database into Amazon S3\. In the next sections, we describe the characteristics of the database\. Then we build the replication resources in AWS DMS that we use to migrate the database, paying close attention to matching the AWS DMS configuration with our particular use case\.

## Example data set<a name="oracle-s3-data-lake-data-set"></a>

For this walkthrough, we use the [Sales History Oracle sample data set](https://docs.oracle.com/en/database/oracle/oracle-database/19/comsc/introduction-to-sample-schemas.html#GUID-5EAB7534-C5BA-47F8-BEFD-7803C078304B)\. The Sales History schema includes 8 tables\. The largest table is the sales table which is a fact table with 5 billion rows\. The total size of this source database is about 200 GB and has about 20 years worth of sales history data in 96 partitions\. The remaining tables are mostly smaller dimension tables\. The following diagram shows the data model for our sample use case\.

![\[A high-level diagram of the source data set.\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-oracle-s3-data-lake-data-set.png)

Note that the costs and sales tables don’t have primary keys\. However, the sales table is partitioned on a date column\. This date column is important to sequence the latest version of a sales record for analysis purposes\.

The company loads data into its data warehouse regularly to gather statistics for these reports\. The company also runs reports on different distribution channels through which its sales are delivered\. When the company runs special promotions on its products, it analyzes the impact of the promotions on sales\. It also analyzes sales by different geographical regions\.

The company in our use case does high volume of business, so it runs business statistics reports and uses machine learning algorithms to aid in decision\-making\. Most of this analysis is time\-sensitive, and they analyze past data trends to get insights on business operations\.

The company’s data scientists want to explore the data to decide which data to use for model training\. Once this data discovery phase is complete, the data will be used to build predictive models using machine learning algorithms\. Once the data is migrated to S3, it is used for training machine learning \(ML\) models using AWS ML managed\-services\. These models will be used for demand product forecasting and inventory replenishment\.

The business also requires that the initial transfer of data from Oracle to Amazon S3 must complete within an 8 hour window\.