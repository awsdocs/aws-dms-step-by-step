# Create and run a migration task<a name="chap-mongodb2documentdb.05"></a>

You are now ready to launch an AWS DMS migration task, to migrate the `zips` data from MongoDB to Amazon DocumentDB\.

1. Open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/](https://console.aws.amazon.com/dms/)\.

1. In the navigation pane, choose **Tasks**\.

1. Choose **Create task** and enter the following information:
   + For **Task name**, enter a name that’s easy to remember, for example `my-dms-task`\.
   + For **Replication instance**, choose the replication instance that you created in [Create an AWS DMS replication instance](chap-mongodb2documentdb.03.md)\.
   + For **Source endpoint**, choose the source endpoint that you created in [Create source and target endpoints](chap-mongodb2documentdb.04.md)\.
   + For **Target endpoint**, choose the target endpoint that you created in [Create source and target endpoints](chap-mongodb2documentdb.04.md)\.
   + For **Migration type**, choose **Migrate existing data**\.
   + For **Start task on create**, enable this option\.

     In the **Task Settings** section, keep all of the options at their default values\.

     In the **Table mappings** section, choose the **Guided** tab, and then enter the following information:
   + For **Schema name is**, choose **Enter a schema**\.
   + For **Schema name is like**, keep this at its default setting \(`%`\)\.
   + For **Table name is like**, keep this at its default setting \(`%`\)\.

   Choose **Add selection rule** to confirm that the information is correct\.

When the settings are as you want them, choose **Create task**\.

AWS DMS now begins migrating data from MongoDB to Amazon DocumentDB\. The task status changes from **Starting** to **Running**\. You can monitor the progress by choosing **Tasks** in the AWS DMS console\. After several minutes, the status changes to **Load complete**\.

**Note**  
After the migration is complete, you can use the mongo shell to connect to your Amazon DocumentDB cluster and view the `zips` data\. For more information, see [Access your Amazon DocumentDB cluster using the mongo shell](https://docs.aws.amazon.com/documentdb/latest/developerguide/getting-started.connect.html) in the *Amazon DocumentDB Developer Guide\.* 