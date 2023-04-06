# Step 7: Create an AWS DMS Replication Instance<a name="chap-rdsoracle2aurora.steps.createreplicationinstance"></a>

After we validate the schema structure between source and target databases, as described preceding, we proceed to the core part of this walkthrough, which is the data migration\. The following illustration shows a high\-level view of the migration process\.

![\[Migration process\]](http://docs.aws.amazon.com/dms/latest/sbs/images/datarep-conceptual2.png)

A DMS replication instance performs the actual data migration between source and target\. The replication instance also caches the transaction logs during the migration\. How much CPU and memory capacity a replication instance has influences the overall time required for the migration\.

To create an AWS DMS replication instance, do the following:

1. Sign in to the AWS Management Console, select [AWS Database Migration Service](https://console.aws.amazon.com/dms/v2) \(AWS DMS\) and choose **Create replication instance**\. If you are signed in as an AWS Identity and Access Management \(IAM\) user, you must have the appropriate permissions to access AWS DMS\. For more information about the permissions required, see [IAM Permissions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Security.html#CHAP_Security.IAMPermissions)\.

1. On the **Create replication instance** page, specify your replication instance information as shown following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.createreplicationinstance.html)

1. For the **Advanced**, **Maintenance**, and **Tags** sections, leave the default settings as they are, and choose **Create**\.