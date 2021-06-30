# Migrating from MongoDB to Amazon DocumentDB<a name="chap-mongodb2documentdb"></a>

Use the following tutorial to guide you through the process of migrating from MongoDB to Amazon DocumentDB \(with MongoDB compatibility\)\. In this tutorial, you do the following:
+ Install MongoDB on an Amazon EC2 instance\.
+ Populate MongoDB with sample data\.
+ Create an AWS DMS replication instance, a source endpoint \(for MongoDB\), and a target endpoint \(for Amazon DocumentDB\)\.
+ Run an AWS DMS task to migrate the data from the source endpoint to the target endpoint\.

**Important**  
Before you begin, make sure to launch an Amazon DocumentDB cluster in your default virtual private cloud \(VPC\)\. For more information, see [Getting started](https://docs.aws.amazon.com/documentdb/latest/developerguide/getting-started.html) in the *Amazon DocumentDB Developer Guide\.* 

**Topics**
+ [Launch an Amazon EC2 instance](chap-mongodb2documentdb.01.md)
+ [Install and configure MongoDB community edition](chap-mongodb2documentdb.02.md)
+ [Create an AWS DMS replication instance](chap-mongodb2documentdb.03.md)
+ [Create source and target endpoints](chap-mongodb2documentdb.04.md)
+ [Create and run a migration task](chap-mongodb2documentdb.05.md)