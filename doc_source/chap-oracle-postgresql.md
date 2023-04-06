# Migrating from Amazon RDS for Oracle to Amazon RDS for PostgreSQL and Aurora PostgreSQL<a name="chap-oracle-postgresql"></a>

 Amazon Relational Database Service \(Amazon RDS\) for PostgreSQL and Amazon Aurora PostgreSQL\-Compatible Edition have evolved as a strong and cost\-effective alternatives to Oracle without the need for a software license or a server to manage\. The journey from Amazon RDS for Oracle to Amazon RDS for PostgreSQL and Aurora PostgreSQL has never been easier\. This guide provides a quick overview of the process and considerations to be made when moving existing workloads to Amazon RDS for PostgreSQL or Aurora PostgreSQL and some of the tools that can assist in the process\. It complements a large body of detailed online reference guidance on every aspects of a migration, and serves to provide a birds eye view of the process\.

This document focuses on migrating custom applications where you control the source code\. If you operate a packaged vendor application on Oracle, you must determine if the vendor supports the new platform\.

 **Topics** 
+  [Can My Oracle Database Migrate?](chap-oracle-postgresql.can-my-db-migrate.md) 
+  [Migration Strategies](#chap-oracle-postgresql.migration-strategies) 
+  [The 12 Step Migration Process](#chap-oracle-postgresql.migration-process) 
  +  [Future State Architecture Design](chap-oracle-postgresql.migration-process.future-state.md) 
  +  [Database Schema Conversion](chap-oracle-postgresql.migration-process.database-schema-conversion.md) 
  +  [Application Conversion or Remediation](chap-oracle-postgresql.migration-process.application-conversion.md) 
  +  [Script/ETL/Report Conversion](chap-oracle-postgresql.migration-process.script-conversion.md) 
  +  [Integration with Third\-Party Applications](chap-oracle-postgresql.migration-process.integration.md) 
  +  [Data Migration Mechanism](chap-oracle-postgresql.migration-process.data-migration.md) 
  +  [Testing and Bug Fixing](chap-oracle-postgresql.migration-process.testing.md) 
  +  [Performance Tuning](chap-oracle-postgresql.migration-process.performance-tuning.md) 
  +  [Setup, DevOps, Integration, Deployment, and Security](chap-oracle-postgresql.migration-process.deployment.md) 
  +  [Documentation and Knowledge Transfer](chap-oracle-postgresql.migration-process.knowledge-transfer.md) 
  +  [Project Management and Version Control](chap-oracle-postgresql.migration-process.project-management.md) 
  +  [Post\-Production Support](chap-oracle-postgresql.migration-process.post-production.md) 
+  [Automation](#chap-oracle-postgresql.automation) 
+  [Platform Differences](chap-oracle-postgresql.platform-differences.md) 

## Migration Strategies<a name="chap-oracle-postgresql.migration-strategies"></a>

The options for dealing with a legacy application have often been described as the 6 R’s\. For more information, see [6 Strategies for Migrating Applications to the Cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)\.
+ Re\-host
+ Re\-platform
+ Repurchase
+ Refactor/Re\-architect
+ Retire
+ Retain

This document describes the steps to migrate database instances running on Amazon RDS for Oracle to Aurora PostgreSQL or Amazon RDS for PostgreSQL\. This also details out the steps to Re\-platform and Refactor the application\(s\) running on these databases\.

Re\-platforming and re\-architecting a database application ranges from modifying the code to work with a different cloud\-native database to also adopting other cloud\-native operations such as serverless application architectures like Kubernetes\. This document deal with the changes necessary to migrate to a new database with pointers to other available documentation\.

## The 12 Step Migration Process<a name="chap-oracle-postgresql.migration-process"></a>

You may have an Oracle database in Amazon RDS for both production or non\-production purposes, and it may just be convenience and familiarity that steered you to Oracle even though there is a licensing cost to this choice\. It is certainly easier to continue with the database you know than something new, but sometimes there are few remaining reasons do so\.

Everyone’s Oracle application is special, and nobody has the same setup and needs for the future\. To provide a single framework for database migrations, this guide organizes the work in 12 steps\. These steps cover what is in scope for most migrations\. You can use these steps in sequence for multiple purposes and you shouldn’t see them as a strictly linear process\. You can consider these steps as an overall arch of a migration project where individual steps and activities can be overlapped or swapped to fit specific project conditions\. The following image shows the 12 steps with an approximate share of effort in a typical project\.

![\[The 12 Step Migration Process\]](http://docs.aws.amazon.com/dms/latest/sbs/images/12-step-migration-process.png)

Each step will be described at a high level in order to allow the reader to skip to relevant topics in the following chapters\.

1.  **Future State Architecture Design** 

   The understanding of the current design and its requirements together with those of the future state are addressed here with deployment diagrams and feature or component mappings for things that will change as a result of the migration\. This step defines the scope and architectural view of the migration\.

1.  **Database Schema Conversion** 

   Because we are migrating a database application from Oracle to Amazon RDS or Aurora, the database schema needs to change\. Subtle differences in functionality and syntax need to accommodate the new platform and comprehensive tooling exists to automate this step\. In this step we include replacements for any Oracle specific database feature which works differently on PostgreSQL\.

1.  **Application Conversion or Remediation** 

   The Oracle application may be implemented in any programming language like Java or C\#, and often abstracts from the nature of the underlying database through an object relational model \(ORM\)\. But it is also common to have some reliance on the database syntax directly in the application code, and this step covers the necessary changes to the application code to work with Amazon RDS for PostgreSQL\. In this step we also include operating system dependencies like direct file access which may need to change on the new platform\.

1.  **Script/ETL/Report Conversion** 

   An Oracle application may move data in and out sideways in addition to the application for the purpose of reporting or data import/export\. This can happen by executing a stored procedure or through external scripting and SQL\*Loader\. Such scripts and PL/SQL statements and the operational framework will need to be modified to work with PostgreSQL\.

1.  **Integration with Third\-Party Applications** 

   Few applications are islands and often connect to other applications and monitoring\. These dependencies may be affected by the move to the PostgreSQL database platform\. The monitoring of the database may need to use native AWS tools or the third\-party applications use Oracle specific means of communication\. These dependencies may already support PostgreSQL or suitable replacements will need to be found\.

1.  **Data Migration Mechanism** 

   As we move from one database platform to another the data needs to move as well\. This will happen several times through the migration, first for testing purposes and later for production cutover\. If there are multiple customers of the database application they may need to be migrated at different times once the application has been migrated\.

1.  **Testing and Bug Fixing** 

   Migration touches all the stored procedures and functions and may affect substantial parts of the application code\. For this reason good testing is required both at the unit and system functional level\.

1.  **Performance Tuning** 

   Due to database platform differences and syntax, certain constructs or combinations of data objects may perform differently on the new platform\. The performance tuning part of the migration resolves any bottlenecks that might have been created\.

1.  **Setup, DevOps, Integration, Deployment, and Security** 

   How the application is put together and deployed may be impacted by the migration, and many customers take the opportunity to embrace infrastructure as code for the first time in the context of a migration\. In this step we also focus on the impact to application security\. In this step we also address cutover planning\.

1.  **Documentation and Knowledge Transfer** 

   In order to support the application going forward it may be necessary to document the changes that happened to the application and the operational environment\. Maintenance of the application will have been impacted by the change of database and certain application behavior may have changed\. This is especially important if the migration is done by a different team from those maintaining the application\.

1.  **Project Management and Version Control** 

   A migration certainly involves people with different skills and often entirely different teams, and maybe an outside party\. A successful project needs to be well planned and coordinated to execute on a predictable schedule\. Version control is a crucial foundation for a migration since database code may not be managed in the same way as application code\.

1.  **Post\-Production Support** 

   After the application is live, the migration team may need to stay around for a while to address any emerging problems on the new platform that were not caught by testing\.

## Automation<a name="chap-oracle-postgresql.automation"></a>

This document references the freely available AWS Schema Conversion Tool \(AWS SCT\) for code conversion and the AWS Database Migration Service for data migration\. For more information, see [Installing, verifying, and updating Schema Conversion Tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Installing.html) and [Database Migration Service](https://aws.amazon.com/dms/)\.