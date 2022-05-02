# Step 1: Create an AWS DMS Replication Instance<a name="chap-rdssqlserver2s3datalake.steps.createreplicationinstance"></a>

To create an AWS Database Migration Service \(AWS DMS\) replication instance, see [Creating a replication instance](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_ReplicationInstance.Creating.html)\. Usually, the full load phase is multi\-threaded \(depending on task configurations\) and has a greater resource footprint than ongoing replication\. Consequently, it’s advisable to start with a larger instance class and then scale down once the task is in the ongoing replication phase\. Moreover, if you intend to migrate your workload using multiple tasks, monitor your replication instance metrics and re\-size your instance accordingly\.

For this use case, we will migrate a subset \(the Sales schema\) of the `AdventureWorks` database, which is over 3 GB in size\. Because we perform a heterogenous migration without many LOB columns, we can start with a compute optimized instance like c5\.xlarge running the latest AWS DMS engine version\. We can later scale up or down based on resource utilization during task execution\.

**Note**  
Scaling replication instance during full load and ongoing replication phases is usually based on CloudWatch metrics such as CPU, memory, I/O, and so on\. Choosing the appropriate replication instance class and size depends on several factors such as number of tasks, table size, DML activity, size of transactions, Large Objects \(LOB\), and so on\. This is out of scope for this walkthrough\. To learn more about these topics, see [Choosing replication instance types](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_ReplicationInstance.Types.html) and [Sizing a replication instance](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_BestPractices.SizingReplicationInstance.html)\.

To create an AWS DMS replication instance, do the following:

1. Sign in to the AWS Management Console, and open the [AWS DMS console](https://console.aws.amazon.com/dms/v2)\.

1. If you are signed in as an AWS Identity and Access Management \(IAM\) user, you must have the appropriate permissions to access AWS DMS\. For more information about the permissions required, see [IAM permissions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Security.html#CHAP_Security.IAMPermissions)\.

1. On the Welcome page, choose **Create replication instance** to start a database migration\.

1. On the **Create replication instance** page, specify your replication instance information\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdssqlserver2s3datalake.steps.createreplicationinstance.html)

1. Choose **Create**\.