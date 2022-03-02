# Create an AWS DMS replication instance<a name="chap-mongodb2documentdb.03"></a>

To perform replication in AWS DMS, you need a replication instance\.

1. Open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. In the navigation pane, choose **Replication instances**\.

1. Choose **Create replication instance** and enter the following information:
   + For **Name**, enter `mongodb2docdb`\.
   + For **Description**, enter `MongoDB to Amazon DocumentDB replication instance`\.
   + For **Instance class**, keep the default value\.
   + For **Engine version**, keep the default value\.
   + For **VPC**, choose your default VPC\.
   + For **Multi\-AZ**, choose **No**\.
   + For **Publicly accessible**, enable this option\.

   When the settings are as you want them, choose **Create replication instance**\.

**Note**  
You can begin using your replication instance when its status becomes **available**\. This can take several minutes\.