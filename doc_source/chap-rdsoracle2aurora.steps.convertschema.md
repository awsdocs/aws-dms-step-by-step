# Step 5: Use the AWS Schema Conversion Tool \(AWS SCT\) to Convert the Oracle Schema to Aurora MySQL<a name="chap-rdsoracle2aurora.steps.convertschema"></a>

Before you migrate data to Aurora MySQL, you convert the Oracle schema to an Aurora MySQL schema\.

To convert an Oracle schema to an Aurora MySQL schema using AWS Schema Conversion Tool \(AWS SCT\), do the following:

1. Launch the AWS Schema Conversion Tool \(AWS SCT\)\. In the AWS SCT, choose **File**, then choose **New Project**\. Create a new project called `DMSDemoProject`\. Enter the following information in the New Project window and then choose **OK**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.convertschema.html)  
![\[Creating a new project in the AWS Schema Conversion Tool\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora10.9.png)

1. Choose **Connect to Oracle**\. In the **Connect to Oracle** dialog box, enter the following information, and then choose **Test Connection**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.convertschema.html)  
![\[Creating a new project in the AWS Schema Conversion Tool\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora11.png)

1. Choose **OK** to close the alert box, then choose OK to close the dialog box and to start the connection to the Oracle DB instance\. The database structure of the Oracle DB instance is shown\. Select only the HR schema\.  
![\[Testing a connection\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora12.png)

1. Choose **Connect to Amazon Aurora**\. In the **Connect to Amazon Aurora** dialog box, enter the following information and then choose **Test Connection**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.convertschema.html)  
![\[Creating a new project in the AWS Schema Conversion Tool\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora12.5.png)

   AWS SCT analyses the HR schema and creates a database migration assessment report for the conversion to Amazon Aurora MySQL\.

1. Choose **OK** to close the alert box, then choose **OK** to close the dialog box to start the connection to the Amazon Aurora MySQL DB instance\.

1. Right\-click the HR schema and select **Create Report**\.  
![\[Database migration report in AWS SCT\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora12.7.png)

1. Check the report and the action items it suggests\. The report discusses the type of objects that can be converted by using AWS SCT, along with potential migration issues and actions to resolve these issues\. For this walkthrough, you should see something like the following:  
![\[Database migration report in AWS SCT\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora13.png)

   You can optionally save the report as \.csv or \.pdf format for later analysis\.

1. Choose the **Action Items** tab, and review any recommendations that you see\.

1. Right\-click the HR schema, and then choose **Convert schema**\.  
![\[Choosing Convert schema in AWS SCT\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora16.png)

1. Choose **Yes** for the confirmation message\. AWS SCT then converts your schema to the target database format\.  
![\[AWS SCT schema conversion\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora17.png)

1. Choose the HR schema, and then choose **Apply to database** to apply the schema scripts to the target Aurora MySQL instance, as shown following\.  
![\[Applying AWS SCT schema scripts\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora18.png)

1. Choose the HR schema, and then choose **Refresh from Database** to refresh from the target database, as shown following\.  
![\[Refreshing from the target database\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora19.png)

The database schema has now been converted and imported from source to target\.