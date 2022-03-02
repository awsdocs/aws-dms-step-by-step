# Database Schema Conversion<a name="chap-oracle-postgresql.migration-process.database-schema-conversion"></a>

Relational databases contain a tabular structure for data using basic data types and procedural code in the form of triggers, functions and stored procedures\. Oracle uses the PL/SQL dialect which is different from the PL/pgSQL dialect of PostgreSQL and while some table definitions and queries may look the same, others will require modification\. Doing so manually would be a substantial task, but fortunately there are freely available AWS tools to automate this job \(See Automation section in the Introduction\)\.

The AWS Schema Conversion Tool \(AWS SCT\) is capable of connecting to Oracle and reading all PL/SQL code directly from the source Oracle database and converting it to PostgreSQL PL/pgSQL\. AWS SCT will retrieve the DDL for tables, views, triggers, stored procedures and functions from the database, parse it and generate the equivalent PostgreSQL code\.

Based on experience, AWS SCT fully converts 90\+% of the database code which leaves less than 10% for the database expert to improve\.

## Process<a name="chap-oracle-postgresql.migration-process.database-schema-conversion.process"></a>

At a high level, the database conversion process works like this:
+ Download and install AWS SCT \(Linux or Windows\)\.
+ Download and install Oracle database drivers \(you probably have those already\)\.
+ Download and install the PostgreSQL database drivers for Amazon RDS or Aurora PostgreSQL\.
+ Run AWS SCT and create a migration assessment report\. For more information, see [Creating migration assessment reports with AWS SCT](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_AssessmentReport.html) in the *AWS Schema Conversion Tool User Guide*\.
+ Run AWS SCT and automatically convert the database code\. For more information, see [Converting database schemas using the AWS SCT](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.html) in the *AWS Schema Conversion Tool User Guide*\.
+ Fix any warnings and error in the database code conversion\.

 AWS SCT operates with default assumptions about mappings between Oracle and PostgreSQL which may or may not be optimal for your particular application due to the data you have in the database\. Certain data type mappings may need to be changed to ensure good performance\. As an example, a NUMBER datatype in Oracle is an extremely versatile container which without further qualification may be too expensive for the application\. In this case you would look at the type of data contained in the NUMERIC column and its requirements for precision and scale, and then determine the best match for that in PostgreSQL with the appropriate precision and scale\.

Once AWS SCT has automatically converted the DDL code, the developer needs to investigate any warnings and errors which need manual remediation\. Warnings and Errors can happen for many reasons\. AWS SCT does not have 100% coverage of all syntactical situations, and code inside the database can be corrupted or encrypted preventing AWS SCT from reading it\. In these situations, the output DDL code is marked up with comments about the problem AWS SCT had with conversion, and ask the developer for help\.

## Exceptions<a name="chap-oracle-postgresql.migration-process.database-schema-conversion.exceptions"></a>

There are exceptions to the automatic code conversion by AWS SCT like SQLJ, \.NET Stored Procedures, Spatial data, RDF Graphs\. But in each case there are good candidate replacement features like Lambda functions, PostGIS and Neptune\.

## Interactive and Batch Modes<a name="chap-oracle-postgresql.migration-process.database-schema-conversion.interactive"></a>

 AWS SCT offers both an interactive GUI and a command line interface \(CLI\) which are useful in different situations\. The user interface is good in a more interactive situations where the user needs to explore the schema and perhaps select only part of it for conversion\. The CLI is good for automation in situations where DDL code might be coming from a different source such as reports\. For more information, see [Script/ETL/Report Conversion](chap-oracle-postgresql.migration-process.script-conversion.md)\.

## Schema Drift<a name="chap-oracle-postgresql.migration-process.database-schema-conversion.schema-drift"></a>

If the original database schema changes during the timeframe of migration, this can be detected in AWS SCT which can compare the old and the new database schema and highlight the object that need to be updated\. If an object was converted 100% or with few manual changes, that object can be converted again and remediated\.

For more information, see [AWS Schema Conversion Tool User Guide](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/Schema-Conversion-Tool.pdf), [Oracle Database 19c To Amazon Aurora with PostgreSQL Compatibility \(12\.4\) Migration Playbook](https://d1.awsstatic.com/whitepapers/Migration/oracle-database-amazon-aurora-postgresql-migration-playbook-12.4.pdf), and [AWS Schema Conversion Tool CLI and Interactive Mode Reference](https://s3.amazonaws.com/publicsctdownload/AWS+SCT+CLI+Reference.pdf)\.