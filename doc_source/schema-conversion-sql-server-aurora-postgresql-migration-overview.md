# Migration Overview<a name="schema-conversion-sql-server-aurora-postgresql-migration-overview"></a>

This section provides high\-level guidance for customers looking to migrate from SQL Server to PostgreSQL using DMS Schema Conversion\.

DMS Schema Conversion automatically converts your source SQL Server database schemas and most of the database code objects to a format compatible with PostgreSQL\. This conversion includes tables, views, stored procedures, functions, data types, synonyms, and so on\. Any objects that DMS Schema Conversion can’t convert automatically are clearly marked\. To complete the migration, you can convert these objects manually\.

At a high level, DMS Schema Conversion operates with the following three components: instance profiles, data providers, and migration projects\. An instance profile specifies network and security settings\. A data provider stores database connection credentials\. A migration project contains data providers, an instance profile, and migration rules\. AWS DMS uses data providers and an instance profile to design a process that converts database schemas and code objects\.

The following diagram illustrates the DMS Schema Conversion process for this walkthrough\.

![\[SQL Server to PostgreSQL migration architecture in DMS Schema Conversion\]](http://docs.aws.amazon.com/dms/latest/sbs/images/schema-conversion-sql-server-aurora-postgresql-migration-architecture.png)

Start the walkthrough by [creating the required resources](schema-conversion-sql-server-aurora-postgresql-step-1.md)\.