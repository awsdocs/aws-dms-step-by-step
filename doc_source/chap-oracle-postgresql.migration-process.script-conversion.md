# Script/ETL/Report Conversion<a name="chap-oracle-postgresql.migration-process.script-conversion"></a>

ETL is an acronym that stands for Extract, Transform and Load\. The ETL process plays a central role in data integration strategies\. ETL allows businesses to gather data from multiple sources and consolidate it into a single, centralized location\. ETL also makes it possible for different types of data to work together\.

ELT is similar to ETL\. However, the primary difference between them is that the data transformation processes occur after the Raw data from the source have been extracted and loaded into a staging area\. The transformation of the data may occur in the destination database or in the middle tier or via serverless tools that might reduce the cost of the data processing\.

Transforming the data is a critical process that may provide significant value to the data\. It’s also the stage where the data could be cleansed, standardized, deduplicated, verified, sorted, shared, and much more\.

The role of ELT or ETL in database migration projects is critical for any successful migration\.

For the remainder of this document, ETL will also refer to ELT patterns\.

ETL can be implemented in the database itself, in external scripts or in third\-party tools such as Informatica, Talend, and so on\. If the ETL is done using Oracle stored procedure, the freely available AWS Schema Conversion Tool \(AWS SCT\) is capable of converting the ETL code to AWS Glue\. For more information, see [Automation](chap-oracle-postgresql.md#chap-oracle-postgresql.automation)\.

## Process for Conversion to \[\.shared\]`GLU`<a name="chap-oracle-postgresql.migration-process.script-conversion.process"></a>

If Python/Glue is a desired future state architecture for ETL code, and the ETL is implemented in the database, the conversion process works like this:

1. Perform the database conversion\. This is necessary because the PL/SQL conversion needs to know the schema of the database\. For more information, see [Database Schema Conversion](chap-oracle-postgresql.migration-process.database-schema-conversion.md)\.

1. Run AWS SCT, select the code involved in ETL and automatically convert the ETL code to AWS Glue\. For more information, see [Converting ETL processes to AWS Glue](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP-converting-aws-glue.html)\.

1. Fix any warnings and errors in the ETL code conversion\.

## Process for Conversion of Stored Procedures<a name="chap-oracle-postgresql.migration-process.script-conversion.process-stored-procedures"></a>

If ETL or report process is implemented in the database, then the database conversion takes care of converting the code, and only the method to call the stored procedures need to change\.

## Process for Conversion of Scripts, Reports, and Third\-Party ETL<a name="chap-oracle-postgresql.migration-process.script-conversion.process-scripts"></a>

If the ETL or Report code is available in scripts or hosted in third\-party tools and those tools will be used in the future, then a custom process will have to be implemented:

1. Perform the database conversion\. This is necessary because the PL/SQL conversion needs to know the schema of the database\. For more information, see [Database Schema Conversion](chap-oracle-postgresql.migration-process.database-schema-conversion.md)\.

1. Extract PL/SQL statements from the third\-party ETL or reporting tool into flat files, unless already available\.

1. Write YAML configuration files for AWS SCT CLI to convert external files\.

1. Run AWS SCT CLI on the external scripts using the YAML configuration files\.For more information, see [AWS Schema Conversion Tool CLI and Interactive Mode Reference](https://s3.amazonaws.com/publicsctdownload/AWS+SCT+CLI+Reference.pdf)\.

1. Fix any warnings and errors in the ETL or report code conversion\.

1. Insert the converted PL/pgSQL code back into the third\-party ETL or reporting tool, unless they stay as flat files\.