# Step 5: Use the AWS SCT to Convert the Oracle Schema to Amazon Redshift<a name="chap-rdsoracle2redshift.steps.convertschema"></a>

Before you migrate data to Amazon Redshift, you convert the Oracle schema to an Amazon Redshift schema\. [This video covers all the steps of this process](https://youtu.be/ZK7J74VJT04)\.

To convert an Oracle schema to an Amazon Redshift schema using AWS Schema Conversion Tool \(AWS SCT\), do the following:

1. Launch the AWS SCT\. In the AWS SCT, choose **File**, then choose **New Project**\. Create a new project named `DWSchemaMigrationDemoProject`, specify the **Location** of the project folder, and then choose **OK**\.

1. Choose **Add source** to add a source Oracle database to your project, then choose **Oracle**, and choose **Next**\.

1. Enter the following information, and then choose **Test Connection**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2redshift.steps.convertschema.html)  
![\[Connecting to an Amazon RDS for Oracle DB instance in the AWS Schema Conversion Tool\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift11.png)

1. Choose **OK** to close the alert box, then choose **Connect** to close the dialog box and to connect to the Oracle DB instance\.

1. Choose **Add target** to add a target Amazon Redshift database to your project, then choose **Amazon Redshift**, and choose **Next**\.

1. Enter the following information and then choose **Test Connection**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2redshift.steps.convertschema.html)

1. Choose **OK** to close the alert box, then choose **Connect** to connect to the Amazon Redshift DB instance\.

1. In the tree in the left panel, select only the **SH** schema\. In the tree in the right panel, select your target Amazon Redshift database\. Choose **Create mapping**\.  
![\[Creating a mapping rule\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift12.png)

1. Choose **Main view**\.

1. In the tree in the left panel, right\-click the **SH** schema and choose **Collect Statistics**\. AWS SCT analyzes the source data to recommend the best keys for the target Amazon Redshift database\. For more information, see [Collecting or Uploading Statistics for the AWS Schema Conversion Tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.DW.html#CHAP_Converting.DW.Statistics)\.
**Note**  
If the **SH** schema does not appear in the list, choose **Actions**, then choose **Refresh from Database**\.

1. In the tree in the left panel, right\-click the **SH** schema and choose **Create report**\. AWS SCT analyzes the **SH** schema and creates a database migration assessment report for the conversion to Amazon Redshift\.

1. Check the report and the action items it suggests\. The report discusses the type of objects that can be converted by using AWS SCT, along with potential migration issues and actions to resolve these issues\. For this walkthrough, you should see something like the following:  
![\[Database migration report in AWS SCT\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift13.png)

1. Review the report summary\. To save the report, choose either **Save to CSV** or **Save to PDF**\.

1. Choose the **Action Items** tab\. The report discusses the type of objects that can be converted by using AWS SCT, along with potential migration issues and actions to resolve these issues\.

1. In the tree in the left panel, right\-click the **SH** schema and choose **Convert schema**\.

1. Choose **Yes** for the confirmation message\. AWS SCT then converts your schema to the target database format\.
**Note**  
The choice of the Amazon Redshift sort keys and distribution keys is critical for optimal performance\. You can use key management in AWS SCT to customize the choice of keys\. For this walkthrough, we use the defaults recommended by AWS SCT\. For more information, see [Optimizing Amazon Redshift by Using the AWS Schema Conversion Tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Converting.DW.RedshiftOpt.html)\.

1. In the tree in the right panel, choose the converted **sh** schema, and then choose **Apply to database** to apply the schema scripts to the target Amazon Redshift instance\.

1. In the tree in the right panel, choose the **sh** schema, and then choose **Refresh from Database** to refresh from the target database\.

The database schema has now been converted and imported from source to target\.