# Step 3: Test Connectivity to the Oracle DB Instance and Create the Sample Schema<a name="chap-rdsoracle2aurora.steps.connectoracle"></a>

After the AWS CloudFormation stack has been created, test the connection to the Oracle DB instance by using SQL Workbench/J and then create the **HR** sample schema\.

To test the connection to your Oracle DB instance and create the sample schema, do the following:

1. In SQL Workbench/J, choose **File**, then choose **Connect window**\. Create a new connection profile using the following information as shown following    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.connectoracle.html)

1. Test the connection by choosing **Test**\. Choose **OK** to close the dialog box, then choose OK to create the connection profile\.  
![\[Connecting to the Oracle DB instance\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora9.png)
**Note**  
If your connection is unsuccessful, ensure that the IP address you assigned when creating the AWS CloudFormation template is the one you are attempting to connect from\. This is the most common issue when trying to connect to an instance\.

1. Create the HR schema you will use for migration using a custom SQL script \(Oracle\-HR\-Schema\-Build\.sql\)\. To obtain this script, do the following:

   1. Download the following archive to your computer: ` [http://docs\.aws\.amazon\.com/dms/latest/sbs/samples/dms\-sbs\-RDSOracle2Aurora\.zip](http://docs.aws.amazon.com/dms/latest/sbs/samples/dms-sbs-RDSOracle2Aurora.zip) ` 

   1. Extract the SQL script\(`Oracle-HR-Schema-Build.sql`\) from the archive\.

   1. Copy and paste the `Oracle-HR-Schema-Build.sql` file into your current directory\.

1. Open the provided SQL script in a text editor\. Copy the entire script\.

1. In SQL Workbench/J, paste the SQL script in the Default\.wksp window showing **Statement 1**\.

1. Choose **SQL**, then choose **Execute All**\.

   When you run the script, you will get an error message indicating that user **HR** does not exist\. You can ignore this error and run the script\. The script drops the user before creating it,which generates the error\.

1. Verify the object types and count in **HR** Schema were created successfully by running the following SQL query\.

   ```
   Select OBJECT_TYPE, COUNT(*) from dba_OBJECTS where owner='HR'
   GROUP BY OBJECT_TYPE;
   ```

   The results of this query should be similar to the following:

   ```
   OBJECT_TYPE    COUNT(*)
   INDEX          8
   PROCEDURE      2
   SEQUENCE       3
   TABLE          7
   VIEW           1
   ```

1. Verify the number of constraints in the **HR** schema by running the following SQL query:

   ```
   Select CONSTRAINT_TYPE,COUNT(*) from dba_constraints  where owner='HR'
   	AND (CONSTRAINT_TYPE IN ('P','R')OR SEARCH_CONDITION_VC NOT LIKE '%NOT NULL%')
   	GROUP BY CONSTRAINT_TYPE;
   ```

   The results of this query should be similar to the following:

   ```
   CONSTRAINT_TYPE	COUNT(*)
   	R	         10
   	P	          7
   	C	          1
   ```

1. Analyze the **HR** schema by running the following:

   ```
   BEGIN
       dbms_stats.gather_schema_stats('HR');
   END;
   /
   ```

1. Verify the total number of tables and number of rows for each table by running the following SQL query:

   ```
   SELECT table_name, num_rows from dba_tables where owner='HR'  order by 1;
   ```

   The results of this query should be similar to the following:

   ```
   TABLE_NAME      NUM_ROWS
   COUNTRIES        25
   DEPARTMENTS      27
   EMPLOYEES       107
   JOBS             19
   JOB_HISTORY      10
   LOCATIONS        23
   REGIONS           4
   ```

1. Verify the relationships of the tables\. Check the departments with employees greater than 10 by running the following SQL query:

   ```
   Select b.department_name,count(*) from HR.Employees a,HR.departments b where a.department_id=b.department_id
   group by b.department_name having count(*) > 10
   order by 1;
   ```

   The results of this query should be similar to the following:

   ```
   DEPARTMENT_NAME      COUNT(*)
   Sales                34
   Shipping             45
   ```