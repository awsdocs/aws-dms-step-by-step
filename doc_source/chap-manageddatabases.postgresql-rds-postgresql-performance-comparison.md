# Performance Comparison<a name="chap-manageddatabases.postgresql-rds-postgresql-performance-comparison"></a>

We analyzed the performance of pg\_dump and pg\_restore, publisher and subscriber, and pglogical in a full load migration\. We migrated a 70 GB database that includes 6 tables and LOB data\. We lifted and shifted this database from the source to the target\.

The following image represents the performance comparison of the three migration methods\. We expect similar performance trends for larger datasets\.

![\[Full load performance comparison\]](http://docs.aws.amazon.com/dms/latest/sbs/images/postgresql-rds-postgresql-performance.png)

We performed this test to provide a basic overview of the full load performance\. This performance may vary because it depends on such factors as network bandwidth, data structure, parallel jobs, data size, and so on\.

The elapsed time shown in the diagram is the actual migration time\. It doesn’t include the time spent on implementing prerequisites\.

You can compare the results of all three methods\.
+ 35 minutes is the total elapsed time for pglogical\.
+ 37 minutes is the total elapsed time for publisher and subscriber\.
+ 46 minutes is the total elapsed time for pg\_dump and pg\_restore\. This time includes:
  + 19 minutes to unload data using pg\_dump\.
  + 27 minutes to load data using pg\_restore\.

From the comparison, you can see that pglogical has the best performance among the three full load options\. Consider this approach if you don’t need to migrate secondary database objects such as views, stored procedures, triggers, and so on\. This is the preferred approach where the database size is greater than 100 GB and when you don’t have transformation or filtering requirements\.

Publisher and subscriber may be appropriate if you don’t need to migrate secondary database objects such as views, stored procedures, triggers, and so on\. You can use publisher and subscriber for smaller migrations where ease of use considerations override the minor performance gains provided by pglogical\.

Using pg\_dump and pg\_restore is slower than both pglogical and publisher and subscriber\. pg\_dump is the only option that migrates your secondary database objects\. Additionally, data files created by pg\_dump may be orders of magnitude larger than the original table size\. You might need a significant amount of storage space when you use pg\_dump to migrate in parallel\.