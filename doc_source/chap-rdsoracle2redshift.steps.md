# Step\-by\-Step Migration<a name="chap-rdsoracle2redshift.steps"></a>

In the following sections, you can find step\-by\-step instructions for migrating an Amazon RDS for Oracle database to Amazon Redshift\. These steps assume that you have already prepared your source database as described in preceding sections\.

**Topics**
+ [Step 1: Launch the RDS Instances in a VPC by Using the AWS CloudFormation Template](chap-rdsoracle2redshift.steps.launchrdswcloudformation.md)
+ [Step 2: Install the SQL Tools and AWS Schema Conversion Tool on Your Local Computer](chap-rdsoracle2redshift.steps.installsct.md)
+ [Step 3: Test Connectivity to the Oracle DB Instance and Create the Sample Schema](chap-rdsoracle2redshift.steps.connectoracle.md)
+ [Step 4: Test the Connectivity to the Amazon Redshift Database](chap-rdsoracle2redshift.steps.connectredshift.md)
+ [Step 5: Use the AWS SCT to Convert the Oracle Schema to Amazon Redshift](chap-rdsoracle2redshift.steps.convertschema.md)
+ [Step 6: Validate the Schema Conversion](chap-rdsoracle2redshift.steps.validateschemaconversion.md)
+ [Step 7: Create an AWS DMS Replication Instance](chap-rdsoracle2redshift.steps.createreplicationinstance.md)
+ [Step 8: Create AWS DMS Source and Target Endpoints](chap-rdsoracle2redshift.steps.createsourcetargetendpoints.md)
+ [Step 9: Create and Run Your AWS DMS Migration Task](chap-rdsoracle2redshift.steps.createmigrationtask.md)
+ [Step 10: Verify That Your Data Migration Completed Successfully](chap-rdsoracle2redshift.steps.verifydatamigration.md)
+ [Step 11: Delete Walkthrough Resources](chap-rdsoracle2redshift.steps.deleteresources.md)