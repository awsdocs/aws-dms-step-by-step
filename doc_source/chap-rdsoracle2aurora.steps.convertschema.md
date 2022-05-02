# Step 5: Use the AWS Schema Conversion Tool to Convert the Oracle Schema to Aurora MySQL<a name="chap-rdsoracle2aurora.steps.convertschema"></a>

Before you migrate data to Aurora MySQL, you convert the Oracle schema to an Aurora MySQL schema\. [This video covers all the steps of this process](https://youtu.be/ClAJUNa1Ucc)\.

To convert an Oracle schema to an Aurora MySQL schema using AWS Schema Conversion Tool \(AWS SCT\), do the following:

1. Launch the AWS SCT\. In the AWS SCT, choose **File**, then choose **New Project**\. Create a new project named `DMSDemoProject`, specify the **Location** of the project folder, and then choose **OK**\.

1. Choose **Add source** to add a source Oracle database to your project, then choose **Oracle**, and choose **Next**\.

1. Enter the following information, and then choose **Test Connection**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.convertschema.html)  
![\[Connecting to an Amazon RDS for Oracle DB instance\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora11.png)

1. Choose **OK** to close the alert box, then choose **Connect** to close the dialog box and to connect to the Oracle DB instance\.

1. Choose **Add target** to add a target Amazon Aurora MySQL database to your project, then choose **Amazon Aurora \(MySQL compatible\)**, and choose **Next**\.

1. Enter the following information and then choose **Test Connection**\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2aurora.steps.convertschema.html)

1. Choose **OK** to close the alert box, then choose **Connect** to connect to the Amazon Aurora MySQL DB instance\.

1. In the tree in the left panel, select only the **HR** schema\. In the tree in the right panel, select your target Aurora MySQL database\. Choose **Create mapping**\.  
![\[Creating a mapping rule\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora12.7.png)

1. Choose **Main view**\. In the tree in the left panel, right\-click the **HR** schema and choose **Create report**\.

1. Check the report and the action items it suggests\. The report discusses the type of objects that can be converted by using AWS SCT, along with potential migration issues and actions to resolve these issues\. For this walkthrough, you should see something like the following:  
![\[Database migration report\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2aurora13.png)

   You can optionally save the report as \.csv or \.pdf format for later analysis\.

1. Choose **Action Items**, and review any recommendations that you see\.

1. In the tree in the left panel, right\-click the **HR** schema and then choose **Convert schema**\.

1. Choose **Yes** for the confirmation message\. AWS SCT then converts your schema to the target database format\.

1. In the tree in the right panel, choose the converted **hr** schema, and then choose **Apply to database** to apply the schema scripts to the target Aurora MySQL instance\.

1. Choose the **hr** schema, and then choose **Refresh from Database** to refresh from the target database\.

The database schema has now been converted and imported from source to target\.