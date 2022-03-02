# Step 10: Verify That Your Data Migration Completed Successfully<a name="chap-rdsoracle2aurora.steps.verifydatamigration"></a>

When the migration task completes, you can compare your task results with the expected results\.

1. On the navigation pane, choose **Tasks**\.

1. Choose your migration task \(`migratehrschema`\)\.

1. Choose the **Table statistics** tab, shown following\.  
![\[Table statistics tab\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora26.png)

1. Connect to the Amazon Aurora MySQL instance by using SQL Workbench/J, and then check if the database tables were successfully migrated from Oracle to Aurora MySQL by running the SQL script shown following\.

   ```
   SELECT TABLE_NAME,TABLE_ROWS
       FROM INFORMATION_SCHEMA.TABLES
       WHERE TABLE_SCHEMA = 'HR' and TABLE_TYPE='BASE TABLE' order by 1;
   ```  
![\[Table statistics tab\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora27.png)

1. Run the following query to check the relationship in tables; this query checks the departments with employees greater than 10\.

   ```
   SELECT B.DEPARTMENT_NAME,COUNT(*)
     FROM HR.EMPLOYEES A,HR.DEPARTMENTS B
     WHERE A.DEPARTMENT_ID=B.DEPARTMENT_ID
     GROUP BY B.DEPARTMENT_NAME HAVING COUNT(*) > 10
     ORDER BY 1;
   ```

   The output from this query should be similar to the following\.

   ```
   department_name	count(*)
   Sales                34
   Shipping             45
   ```

Now you have successfully completed a database migration from an Amazon RDS for Oracle database instance to Amazon Aurora MySQL\.