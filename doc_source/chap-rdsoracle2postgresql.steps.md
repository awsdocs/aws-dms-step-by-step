# Step\-by\-Step Migration<a name="chap-rdsoracle2postgresql.steps"></a>

The following steps provide instructions for migrating an Oracle database to a PostgreSQL database\. These steps assume that you have already prepared your source database as described in [Prerequisites](chap-rdsoracle2postgresql.prerequisites.md)\.

**Topics**
+ [Step 1: Install the SQL Drivers and AWS Schema Conversion Tool on Your Local Computer](chap-rdsoracle2postgresql.steps.installsct.md)
+ [Step 2: Configure Your Oracle Source Database](chap-oracle2postgresql.steps.configureoracle.md)
+ [Step 3: Configure Your PostgreSQL Target Database](chap-oracle2postgresql.steps.configurepostgresql.md)
+ [Step 4: Use AWS SCT to Convert the Oracle Schema to PostgreSQL](chap-rdsoracle2postgresql.steps.convertschema.md)
+ [Step 5: Create an AWS DMS Replication Instance](chap-rdsoracle2postgresql.steps.createreplicationinstance.md)
+ [Step 6: Create AWS DMS Source and Target Endpoints](chap-rdsoracle2postgresql.steps.createsourcetargetendpoints.md)
+ [Step 7: Create and Run Your AWS DMS Migration Task](chap-rdsoracle2postgresql.steps.createmigrationtask.md)
+ [Step 8: Cut Over to PostgreSQL](chap-rdsoracle2postgresql.steps.cutover.md)