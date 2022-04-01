# Migrating from SAP ASE to Amazon Aurora MySQL<a name="chap-sap-ase-aurora-mysql"></a>

Following, you can find a high\-level outline and a step\-by\-step walkthrough that show the migration process of an on\-premises SAP ASE database to Amazon Aurora MySQL\-Compatible Edition using AWS Database Migration Service \(AWS DMS\)\. Amazon Aurora is a highly available and managed relational database service with automatic scaling and high\-performance features\. The combination of MySQL compatibility with Aurora enterprise database capabilities provides an ideal target for commercial database migrations\.

This walkthrough covers all steps in the migration from initial analysis of the source database to final cutover of applications to the target database\.

The following diagram shows the basic architecture for the migration\.

![\[Architecture diagram for SAP ASE migration to Amazon Aurora MySQL\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sap-ase-to-aurora-mysql-architecture-diagram.png)

We use the **pubs2** database for SAP ASE as the example database in the rest of this document\.

**Topics**
+ [Prerequisites](chap-sap-ase-aurora-mysql.prerequisites.md)
+ [Preparation and Assessment](chap-sap-ase-aurora-mysql.assessment.md)
+ [Database Migration](chap-sap-ase-aurora-mysql.migration.md)
+ [Best Practices](chap-sap-ase-aurora-mysql.bestpractices.md)