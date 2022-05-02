# Create and run a migration task<a name="chap-mongodb2documentdb.05"></a>

You are now ready to launch an AWS DMS migration task, to migrate the `zips` data from MongoDB to Amazon DocumentDB\.

1. Open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. In the navigation pane, choose **Database migration tasks**\.

1. Choose **Create task** and enter the following information:
   + For **Task configuration**, choose the following settings:
     +  **Task identifier** — enter a name that’s easy to remember, for example `my-dms-task`\.
     +  **Replication instance** — choose the replication instance that you created in [Create a replication instance](chap-mongodb2documentdb.03.md)\.
     +  **Source database endpoint** — choose the source endpoint that you created in [Create source and target endpoints](chap-mongodb2documentdb.04.md)\.
     +  **Target database endpoint** — choose the target endpoint that you created in [Create source and target endpoints](chap-mongodb2documentdb.04.md)\.
     +  **Migration type** — choose **Migrate existing data**\.
   + For **Task settings**, choose the following settings:
     +  **Target table preparation mode** — Do nothing
     +  **Include LOB columns in replication** — Limited LOB mode
     +  **Maximum LOB size \(KB\)** — 32
     +  **Enable validation** 
     +  **Enable CloudWatch logs** 
**Note**  
CloudWatch logs usage will be charged at standard rates\. See [here](https://aws.amazon.com/cloudwatch/pricing/) for more details\.
   + For **Advanced task settings**, keep all of the options at their default values\.
   + For **Premigration assessment**, keep the option at its default value\.
   + For **Start migration task** in **Migration task startup configuration**, choose **Automatically on create**\.
   + For **Tags**, keep all of the options at their default values\.

When the settings are as you want them, choose **Create task**\.

 AWS DMS now begins migrating data from MongoDB to Amazon DocumentDB\. The task status changes from **Starting** to **Running**\. You can monitor the progress by choosing **Tasks** in the AWS DMS console\. After several minutes, the status changes to **Load complete**\.

**Note**  
After the migration is complete, you can use the mongo shell to connect to your Amazon DocumentDB cluster and view the `zips` data\. For more information, see [Access your Amazon DocumentDB cluster using the mongo shell](https://docs.aws.amazon.com/documentdb/latest/developerguide/getting-started.connect.html) in the *Amazon DocumentDB Developer Guide\.* 