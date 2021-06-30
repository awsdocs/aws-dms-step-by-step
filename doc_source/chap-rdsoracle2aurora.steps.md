# Step\-by\-Step Migration<a name="chap-rdsoracle2aurora.steps"></a>

In the following sections, you can find step\-by\-step instructions for migrating an Amazon Relational Database Service \(Amazon RDS\) Oracle database to Amazon Aurora MySQL\. These steps assume that you have already prepared your source database as described in preceding sections\.

**Topics**
+ [Step 1: Launch the RDS Instances in a VPC by Using the CloudFormation Template](chap-rdsoracle2aurora.steps.launchrdswcloudformation.md)
+ [Step 2: Install the SQL Tools and AWS Schema Conversion Tool on Your Local Computer](chap-rdsoracle2aurora.steps.installsct.md)
+ [Step 3: Test Connectivity to the Oracle DB Instance and Create the Sample Schema](chap-rdsoracle2aurora.steps.connectoracle.md)
+ [Step 4: Test the Connectivity to the Aurora MySQL DB Instance](chap-rdsoracle2aurora.steps.connectaurora.md)
+ [Step 5: Use the AWS Schema Conversion Tool \(AWS SCT\) to Convert the Oracle Schema to Aurora MySQL](chap-rdsoracle2aurora.steps.convertschema.md)
+ [Step 6: Validate the Schema Conversion](chap-rdsoracle2aurora.steps.validateschemaconversion.md)
+ [Step 7: Create a AWS DMS Replication Instance](chap-rdsoracle2aurora.steps.createreplicationinstance.md)
+ [Step 8: Create AWS DMS Source and Target Endpoints](chap-rdsoracle2aurora.steps.createsourcetargetendpoints.md)
+ [Step 9: Create and Run Your AWS DMS Migration Task](chap-rdsoracle2aurora.steps.createmigrationtask.md)
+ [Step 10: Verify That Your Data Migration Completed Successfully](chap-rdsoracle2aurora.steps.verifydatamigration.md)
+ [Step 11: Delete Walkthrough Resources](chap-rdsoracle2aurora.steps.deleteresources.md)