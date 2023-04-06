# Database Migration Step\-by\-Step Walkthroughs<a name="dms-sbs-welcome"></a>

You can use AWS Database Migration Service \(AWS DMS\) to migrate your data to and from most widely used commercial and open\-source databases such as Oracle, PostgreSQL, Microsoft SQL Server, Amazon Redshift, Amazon Aurora, MariaDB, and MySQL\. The service supports homogeneous migrations such as Oracle to Oracle, and also heterogeneous migrations between different database platforms, such as Oracle to MySQL or MySQL to Amazon Aurora MySQL\-Compatible Edition\. Alternatively, you can use AWS DMS to move from existing, self\-managed, open\-source, and commercial databases to fully managed AWS databases of the same engine\.

You can use DMS Schema Conversion to migrate to a different database engine\. This service automatically assesses and converts your source schemas to a new target engine\. Alternatively, you can download the AWS Schema Conversion Tool AWS SCT to your local PC to convert your source schemas\.

In this guide, you can find step\-by\-step walkthroughs that go through the process of schema conversion and data migration of the following source databases:

## Oracle Database<a name="oracle-database"></a>
+  [Migrating an On\-Premises Oracle Database to Amazon Aurora MySQL](chap-on-premoracle2aurora.md) 
+  [Migrating an Amazon RDS for Oracle Database to Amazon Aurora MySQL](chap-rdsoracle2aurora.md) 
+  [Migrating an Amazon RDS for Oracle Database to an Amazon S3 Data Lake](oracle-s3-data-lake.md) 
+  [Migrating an Oracle Database to PostgreSQL](chap-rdsoracle2postgresql.md) 
+  [Migrating Oracle databases to Amazon Aurora MySQL with DMS Schema Conversion](schema-conversion-oracle-aurora-mysql.md) 
+  [Migrating Oracle databases to Amazon RDS for PostgreSQL with DMS Schema Conversion](schema-conversion-oracle-postgresql.md) 
+  [Migrating an Amazon RDS for Oracle Database to Amazon Redshift](chap-rdsoracle2redshift.md) 
+  [Migrating an Oracle Database to Amazon RDS for Oracle](chap-manageddatabases.oracle2rds.md) 
+  [Migrating from Amazon RDS for Oracle to Amazon RDS for PostgreSQL and Aurora PostgreSQL](chap-oracle-postgresql.md) 

## Microsoft SQL Server<a name="microsoft-sql-server"></a>
+  [Migrating a SQL Server Database to Amazon Aurora MySQL](chap-sqlserver2aurora.md) 
+  [Migrating an Amazon RDS for SQL Server Database to an Amazon S3 Data Lake](chap-rdssqlserver2s3datalake.md) 
+  [Migrating SQL Server databases to Amazon Aurora PostgreSQL with DMS Schema Conversion](schema-conversion-sql-server-aurora-postgresql.md) 
+  [Migrating SQL Server databases to Amazon RDS for MySQL with DMS Schema Conversion](schema-conversion-sql-server-mysql.md) 
+  [Migrating a SQL Server Always On Database](chap-manageddatabases.sqlserveralwayson.md) 
+  [Migrating SQL Server Databases to Amazon RDS for SQL Server](chap-manageddatabases.sql-server-rds-sql-server.md) 

## MySQL<a name="mysql"></a>
+  [Migrating MySQL\-Compatible Databases](chap-mysql.md) 
+  [Migrating a MySQL\-Compatible Database to Amazon Aurora MySQL](chap-mysql2aurora.md) 
+  [Migrating a MySQL Database to Amazon RDS for MySQL or Amazon Aurora MySQL](chap-manageddatabases.mysql2rds.md) 

## BigQuery<a name="bigquery"></a>
+  [Migrating a BigQuery Project to Amazon Redshift](bigquery-redshift.md) 

## MariaDB<a name="mariadb"></a>
+  [Migrating a MariaDB Database to Amazon RDS for MySQL or Amazon Aurora MySQL](chap-mariadb2auroramysql.md) 

## MongoDB<a name="mongodb"></a>
+  [Migrating from MongoDB to Amazon DocumentDB](chap-mongodb2documentdb.md) 

## PostgreSQL<a name="postgresql"></a>
+  [Migrating PostgreSQL Databases to Amazon RDS for PostgreSQL or Amazon Aurora PostgreSQL](chap-manageddatabases.postgresql-rds-postgresql.md) 

## SAP ASE<a name="sap-ase"></a>
+  [Migrating from SAP ASE to Amazon Aurora MySQL](chap-sap-ase-aurora-mysql.md) 

In the DMS User Guide, you can find additional resources:
+  [Migrating large data stores](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_LargeDBs.html) 