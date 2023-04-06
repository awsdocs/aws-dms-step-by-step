# Step 2: Configure a Source Amazon RDS for Oracle Database<a name="oracle-s3-data-lake-step-2"></a>

In this step, we configure the source Oracle database\. Make sure that AWS DMS can access the database transaction logs and capture the data changes\. Also, make sure that you set permissions for AWS DMS to access tables and database catalogs\.

 AWS DMS can help users to migrate historical data from Oracle source database and also replicate the ongoing changes to a centralized data lake\. To use Oracle as a source for AWS DMS, you must turn on the `ARCHIVELOG MODE`\.

Make sure that your database server retains the archive logs as long as AWS DMS requires them\. If you configure your task to begin capturing changes immediately, then you should only need to retain archive logs for a little longer than the duration of the longest running transaction\. Retaining archive logs for 24 hours is usually sufficient\. If you configure your task to begin from a point in time in the past, then archive logs must be available from that time forward\. For more information about turning on the `ARCHIVELOG MODE` and ensuring log retention for your Oracle database, see the [Oracle documentation](http://docs.oracle.com/database/121/ADMIN/archredo.htm#ADMIN11335)\.

To capture change data, AWS DMS requires that you turn on supplemental logging on your source database\. Minimal supplemental logging must be turned on at the database level\. AWS DMS also requires that you turn on identification key logging\. This option causes the database to place all columns of a row’s primary key in the redo log file whenever you update a row that contains a primary key\. This occurs even if there is a change of value in any of the columns other than the primary key columns\. You can set this option at the database or table level\.
+ Create or configure a database user AWS DMS\. We recommend that you use a user with the minimal privileges required by AWS DMS for your connection\. AWS DMS requires the following privileges\.

  ```
  CREATE SESSION
  SELECT ANY TRANSACTION
  SELECT on V_$ARCHIVED_LOG
  SELECT on V_$LOG
  SELECT on V_$LOGFILE
  SELECT on V_$DATABASE
  SELECT on V_$THREAD
  SELECT on V_$PARAMETER
  SELECT on V_$NLS_PARAMETERS
  SELECT on V_$TIMEZONE_NAMES
  SELECT on V_$TRANSACTION
  SELECT on ALL_INDEXES
  SELECT on ALL_OBJECTS
  SELECT on ALL_TABLES
  SELECT on ALL_USERS
  SELECT on ALL_CATALOG
  SELECT on ALL_CONSTRAINTS
  SELECT on ALL_CONS_COLUMNS
  SELECT on ALL_TAB_COLS
  SELECT on ALL_IND_COLUMNS
  SELECT on ALL_LOG_GROUPS
  SELECT on SYS.DBA_REGISTRY
  SELECT on SYS.OBJ$
  SELECT on DBA_TABLESPACES
  SELECT on ALL_TAB_PARTITIONS
  SELECT on ALL_ENCRYPTED_COLUMNS
  SELECT on <<all tables migrated>>
  ```
+ For tables with primary key, turn on supplemental logging at key level\.

  ```
  ALTER TABLE table_name ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;
  ```
+ Data warehouse databases can have fact tables without a primary key\. In our sample database, the `SALES` table doesn’t have a primary key\. This table is part of a full load and CDC task, however it does not have primary key or unique indexes\. So we can add supplemental logging on all columns of the table\.

  ```
  ALTER TABLE SH.SALES ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;
  ```

For more information, see [Working with an Oracle database as a source](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Source.Oracle.html#CHAP_Source.Oracle.Amazon-Managed)\.