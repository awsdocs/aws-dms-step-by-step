# Step 8: Run Your Migration Task<a name="bigquery-redshift-migration-step-8"></a>

After you install and configure the data extraction agent, register it in AWS SCT\.

 **To register a data extraction agent** 

1. In AWS SCT, for **View** choose **Data migration view \(other\)**, and then choose **Register**\.

1. For **Description**, enter a name for your data extraction agent\.

1. For **Host name**, enter `0.0.0.0` because you run the data extraction agent on the same computer as AWS SCT\. If you install the data extraction agent on another computer, enter the IP address of this computer\.

1. For **Port**, enter `8192`\. If you configured another listening port in the previous step, use the value that you configured\.

1. Choose **Register**\.

 AWS SCT now can use the data extraction agent for data migration tasks\.

When you migrate big tables, you can split data into virtual partitions in AWS SCT\. Then AWS SCT creates subtasks for each virtual partition\.

 **To create virtual partitions for your table in AWS SCT ** 

1. In the tree in the left panel, choose your source table\. Open the context \(right\-click\) menu, and choose **Add virtual partitioning**\.

1. For **Partition type**, choose **Range**\.

1. For **Column name**, choose the column of your table\. AWS SCT partitions data based on a range of column values\. This partition type creates a `WHERE` clause, and you provide the range of values for each partition\.

1. For **Values**, enter a list of values for the partitioned column\.

1. Choose **OK** to create virtual partitions for your table\.

Now, you can start the data migration\.

 **To create and run a migration task** 

1. In the tree in the left panel, choose your source table\. Open the context \(right\-click\) menu, and choose **Create local task**\.

1. For **Task name**, enter a descriptive name for your data migration task\.

1. For **Migration mode**, choose **Extract, upload, and copy**\.

1. Choose **Advanced**\. For **Google CS bucket folder**, enter the name for your Cloud Storage bucket that you created in [Step 3](bigquery-redshift-migration-step-3.md)\.

1. Choose **Amazon S3 settings**\. For **Amazon S3 bucket folder**, enter the name of your Amazon S3 bucket that you created in [Step 3](bigquery-redshift-migration-step-3.md)\.

1. Choose **Create** and then choose **Start**\.

The AWS SCT data extraction agents migrates data from your BigQuery dataset to Amazon Redshift\. You can manage the migration process in AWS SCT\. After the data extraction agent completes the migration, check your data in Amazon Redshift\. Make sure that all your source data migrated to the new target database\.