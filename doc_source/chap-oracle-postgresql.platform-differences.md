# Platform Differences<a name="chap-oracle-postgresql.platform-differences"></a>

This section discusses some of the differences between Oracle and PostgreSQL to illustrate opportunities and challenges with migrating an Oracle application\. This overview is by no means an exhaustive, however these are common challenges you may encounter when administering PostgreSQL after a background with Oracle\.

## Range and List Partitions<a name="chap-oracle-postgresql.platform-differences.range"></a>

Along with possible performance difference, architecturally partitions on Oracle and PostgreSQL act quite differently\. On Oracle you can define a Range, for example each month or year being a partition, or a list, where every occurrence of say the letter “N” or “Y” in a char field is partitioned at the table definition level and PostgreSQL handles these operations differently\. PostgreSQL operates with a “parent” table that holds no data and a “child” table that defines the partitions themselves and holds the data\. The parent table is created first, the child tables is then defined with corresponding constraints to create the partition\. You must supply a trigger to insert into the parent table and have the data be routed to the correct partition\. For more information, see [Strategy for Migrating Partitioned Tables from Oracle to Amazon RDS for PostgreSQL and Amazon Aurora with PostgreSQL Compatibility](https://aws.amazon.com/blogs/database/strategy-for-migrating-partitioned-tables-from-oracle-to-amazon-rds-postgresql-and-amazon-aurora-postgresql/)\.

## Data Types<a name="chap-oracle-postgresql.platform-differences.data-types"></a>

Some PostgreSQL data types are much easier to work with than their corresponding Oracle types\. For example, the Text type can store up to 1 GB of text and can be handled in SQL just like the char and varchar fields\. They don’t require special large object functions like character large objects \(CLOBs\) do\.

However, there are some important differences to note\. The Numeric field in PostgreSQL can be used to map any Number data types\. But when it’s used for joins \(such as for a foreign key\), it is less performant than using an int or bigint\. This is a typical area where custom data type mapping should be considered\.

The PostgreSQL Timestamp with time zone field is slightly different from and corresponds to the Oracle Timestamp with local time zone\. These small differences can cause either performance issues or subtle application bugs that require thorough testing\.

For more information, see [Migration tips for developers converting Oracle and SQL Server code to PostgreSQL](https://aws.amazon.com/blogs/database/code-conversion-challenges-while-migrating-from-oracle-or-microsoft-sql-server-to-postgresql/)\.

## Transaction Control and Exception Handling<a name="chap-oracle-postgresql.platform-differences.transaction-control"></a>

PostgreSQL Multiversion Concurrency Control \(MVCC\) is very different from Oracle rollback segments, even though they both provide ACID transactions\. PostgreSQL creates a snapshot state taken at the start of the transaction, and essentially copies the data to a temporary page while the transaction is in flight\. This can affect both the queries and the application, as well as hardware considerations\. Because open transactions require temporary space to hold their snapshot state during the transaction, a workload that requires many open transactions in addition to possible deadlocks, needs to consider the location and parameters surrounding temporary table space\.

Unlike Oracle, PostgreSQL uses auto\-commit for transactions by default\. However, there are two options to support explicit transactions, which are similar to the default behavior in Oracle \(non\-auto\-commit\)\. You can use the `START TRANSACTION` or `BEGIN TRANSACTION` statements and then `COMMIT` or `ROLLBACK`; or you can simply set `AUTOCOMMIT` to `OFF` at the session or system level\.

PostgreSQL does not allow transaction control inside of PL/pgSQL like commit or roll back inside a stored procedure\. The caller must perform the transaction management\. If your existing PL/SQL contains explicit commit and rollback code it must be modified\. When a run\-time exception has occurred in a transaction, you must roll back that transaction before you can execute any new statement on the connection\. Finally, exception handling in PL/pgSQL, using a `BEGIN…EXCEPTION…END` block to let your code catch any errors that occur\. This block automatically creates a savepoint before the block, and rolls back to that savepoint when an exception occurs\. You can then determine what logic to execute based on whether there was an error\. Exception blocks are expensive however, due to the created savepoint\. If you don’t need to catch an error, or if you are planning to simply raise the error back to the calling application, don’t use the exception block at all\. Let the original error flow up to the application\.

For more information, see [Oracle Database 19c To Amazon Aurora with PostgreSQL Compatibility \(12\.4\) Migration Playbook](https://d1.awsstatic.com/whitepapers/Migration/oracle-database-amazon-aurora-postgresql-migration-playbook-12.4.pdf)\.