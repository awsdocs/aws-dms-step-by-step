# Step 6: Validate the Schema Conversion<a name="chap-rdsoracle2aurora.steps.validateschemaconversion"></a>

To validate the schema conversion, you compare the objects found in the Oracle and Aurora MySQL databases using SQL Workbench/J\.

1. In SQL Workbench/J, choose **File**, then choose **Connect window**\. Choose the RDSAuroraConnection you created in an earlier step\. Click **OK**\.

1. Run the following script to verify the number of object types and count in the **HR** schema in the target Aurora MySQL database\. These values should match the number of objects in the source Oracle database:

   ```
   SELECT a.OBJECT_TYPE, COUNT(*)
   FROM
   (
   SELECT OBJECT_TYPE
   ,OBJECT_SCHEMA
   ,OBJECT_NAME
   FROM (
   SELECT 'TABLE' AS OBJECT_TYPE
   ,TABLE_NAME AS OBJECT_NAME
   ,TABLE_SCHEMA AS OBJECT_SCHEMA
   FROM information_schema.TABLES
   where  TABLE_TYPE='BASE TABLE'
   UNION
   SELECT 'VIEW' AS OBJECT_TYPE
   ,TABLE_NAME AS OBJECT_NAME
   ,TABLE_SCHEMA AS OBJECT_SCHEMA
   FROM information_schema.VIEWS
   UNION
   
   SELECT 'INDEX' AS OBJECT_TYPE
   ,CONCAT (
   CONSTRAINT_TYPE
   ,' : '
   ,CONSTRAINT_NAME
   ,' : '
   ,TABLE_NAME
   ) AS OBJECT_NAME
   ,TABLE_SCHEMA AS OBJECT_SCHEMA
   FROM information_schema.TABLE_CONSTRAINTS
   where constraint_type='PRIMARY KEY'
   UNION
   SELECT ROUTINE_TYPE AS OBJECT_TYPE
   ,ROUTINE_NAME AS OBJECT_NAME
   ,ROUTINE_SCHEMA AS OBJECT_SCHEMA
   FROM information_schema.ROUTINES
   UNION
   SELECT 'TRIGGER' AS OBJECT_TYPE
   ,CONCAT (
   TRIGGER_NAME
   ,' : '
   ,EVENT_OBJECT_SCHEMA
   ,' : '
   ,EVENT_OBJECT_TABLE
   ) AS OBJECT_NAME
   ,TRIGGER_SCHEMA AS OBJECT_SCHEMA
   FROM information_schema.triggers
   ) R
   WHERE R.OBJECT_SCHEMA ='HR'
   order by 1) a
   GROUP BY a.OBJECT_TYPE;
   ```

   The output from this query should be similar to the following:

   ```
   OBJECT_TYPE    COUNT(*)
   INDEX           7
   PROCEDURE       2
   TABLE           7
   TRIGGER        10
   VIEW            1
   ```

   Next, run the following query to get table constraints information:

   ```
   SELECT CONSTRAINT_TYPE,COUNT(*)
   FROM information_schema.TABLE_CONSTRAINTS where constraint_schema='HR'
   GROUP BY CONSTRAINT_TYPE;
   ```

   The output from this query should be similar to the following:

   ```
   CONSTRAINT_TYPE    COUNT(*)
   FOREIGN KEY        10
   PRIMARY KEY         7
   UNIQUE              7
   ```