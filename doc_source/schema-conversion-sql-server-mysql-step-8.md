# Step 8: Convert Database Objects<a name="schema-conversion-sql-server-mysql-step-8"></a>

After you create the migration project, you can convert your SQL Server database schemas to MySQL\. To start working with your migration project, you launch DMS Schema Conversion\.

The first launch of DMS Schema Conversion requires some setup\. AWS Database Migration Service \(AWS DMS\) starts a schema conversion instance, which can take 10\-15 minutes\. This process also reads the metadata from the source and target databases\. After a successful first launch, you can access DMS Schema Conversion instantly\.

 **To convert your source SQL Server database schema with DMS Schema Conversion** 

1. Sign in to the AWS Management Console and open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/v2/](https://console.aws.amazon.com/dms/v2/)\.

1. Choose your AWS Region\.

1. Choose **Migration projects**\. The **Migration projects** page opens\.

1. Choose `sc-project`, and then choose **Schema conversion**\.

1. Choose **Launch schema conversion**\. If you launch schema conversion for the first time, then the notification appears\. Choose **Launch**\. The **Schema conversion** page opens\. DMS Schema Conversion displays your source database schema in the left pane in a tree\-view format\.

1. In the source database pane, select the check box for the schema name\.

1. Choose this schema in the left pane of the migration project\. DMS Schema Conversion highlights the schema name in blue and activates the **Actions** menu\.

1. For **Actions**, choose **Convert schema**\. The conversion dialog box appears\.

1. Choose **Convert** in the dialog box to confirm your choice\.

After DMS Schema Conversion completes the conversion, you can review the converted code\. After you choose a database object in the left pane of your project, DMS Schema Conversion automatically displays the source converted code for this object\.

DMS Schema Conversion stores the converted code in your migration project and doesn’t apply these code changes to your target database\. You can apply the converted code in DMS Schema Conversion\. Alternatively, you can save the converted code as a SQL script, edit it, and then apply to your target database\. For more information, see [Step 9](schema-conversion-sql-server-mysql-step-9.md)\.

In the settings of your migration project, you can customize your schema conversion view\. Also, you can change conversion settings to improve the performance of converted code\.

 **To edit the settings of your DMS Schema Conversion migration project** 

1. In the AWS DMS console, choose **Migration projects**\. The **Migration projects** page opens\.

1. Choose your migration project\. Choose **Schema conversion**, then **Launch schema conversion**\.

1. Choose **Settings**\. The **Settings** page opens\.

1. Change the settings to customize the schema conversion view\. For more information, see [Specifying migration project settings](https://docs.aws.amazon.com/dms/latest/userguide/migration-projects-settings.html)\.

1. Change the settings to improve the performance of converted code\. For more information, see [Specifying SQL Server to MySQL conversion settings](https://docs.aws.amazon.com/dms/latest/userguide/schema-conversion-sql-server-mysql.html)\.

1. Choose **Apply**, and then choose **Schema conversion**\.

After you change the settings, convert your source code again\.