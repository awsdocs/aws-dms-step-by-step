# Application Conversion or Remediation<a name="chap-oracle-postgresql.migration-process.application-conversion"></a>

The Oracle application may be written in a variety of languages like C\+\+, C\# and Java, each with their own patterns for calling Oracle\. A common case is the use of an object relational model \(ORM\) between the application code and the database which reduce the amount of PL/SQL that needs to be changed\. Examples include Entity Framework and Hibernate which are also supported on PostgreSQL\.

Oracle uses the PL/SQL dialect which is different from the PL/pgSQL dialect of PostgreSQL and while some table definitions and queries may look the same, others will require modification\. Doing so manually would be a substantial task, but the freely available AWS Schema Conversion Tool \(AWS SCT\)\.

 AWS SCT is capable of identifying and replacing embedded PL/SQL in the application code with the equivalent PostgreSQL code\. For more information, see [Automation](chap-oracle-postgresql.md#chap-oracle-postgresql.automation)\.

In addition to using AWS SCT, you must also examine the source code for possible issues like:
+ Specific ORM or other data access framework and versions or in use and confirm its compatibility with the target engine\.
+ Modify database connection as appropriate for the new engine\.
+ Modify any table/entity mapping configuration or code as appropriate for the converted schema\.
+ Identify and refactor any vendor\-specific driver functionality in use in the code\.

## Process<a name="chap-oracle-postgresql.migration-process.application-conversion.process"></a>

At a high level, the application conversion process works like this:

1. Perform the database conversion\. This is necessary because the PL/SQL conversion needs to know the schema of the database\. For more information, see [Database Schema Conversion](chap-oracle-postgresql.migration-process.database-schema-conversion.md)\.

1. Run AWS SCT and automatically convert the application code\. For more information, see [Converting application SQL](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.App.html)\.

1. Fix any warnings and errors in the application code conversion\.

## Exceptions<a name="chap-oracle-postgresql.migration-process.application-conversion.exceptions"></a>

There are exceptions to the automated application code conversion process If the application uses the native Oracle Call Interface \(OCI\)\. In this case the developer must refactor the code to use ODBC or JDBC\.