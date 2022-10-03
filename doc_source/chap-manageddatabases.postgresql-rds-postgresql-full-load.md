# Full Load<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load"></a>

The full load migration phase populates the target database with a copy of the source data\. This chapter describes the following native methods to help you choose the one that best matches your migration scenario\.
+ pg\_dump and pg\_restore
+ Publisher and Subscriber
+ pglogical

We recommend that you begin by reviewing the following table to understand the tools suitable for your use case\.


| Method | Supported versions | Support of metadata migration | Suitable database sizes | Performance | 
| --- | --- | --- | --- | --- | 
|  pg\_dump and pg\_restore  |  All versions of PostgreSQL  |  Yes  |  100 GB or less  |  Medium  | 
|  Publisher and Subscriber  |  PostgreSQL 10\.0 and higher  |  No  |  Any size  |  High  | 
|  pglogical  |  PostgreSQL 9\.4 and higher  |  Yes  |  Any size  |  High  | 

The suitable database sizes provided in the preceding table are the AWS DMS recommendations\. These recommendations are based on customer migration experiences and aren’t the limitation of the native tools\.

For more information about the performance of these tools, see [Performance Comparison](chap-manageddatabases.postgresql-rds-postgresql-performance-comparison.md)\.

**Topics**
+ [Preparing for Ongoing Replication](chap-manageddatabases.postgresql-rds-postgresql-full-load-preparing.md)
+ [pg\_dump and pg\_restore](chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump.md)
+ [Publisher and Subscriber](chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher.md)
+ [pglogical](chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical.md)