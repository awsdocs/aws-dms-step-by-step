# Step 9: Edit and Apply Your Converted Code<a name="schema-conversion-sql-server-mysql-step-9"></a>

After you convert your source SQL Server database objects, you can review the conversion statistics\. DMS Schema Conversion converts most of the database objects, but some of the objects require manual conversion\.

DMS Schema Conversion displays the objects that require manual conversion in the **Action items** tab\. To convert these objects, you can save the converted code as a SQL script\. Then you can edit it using your code editor and apply these scripts to your target database\. Alternatively, you can apply the converted code as is to your target database and make the edits later\.

 **To save the converted code as a SQL script** 

1. In the target database pane, choose the converted database schema\.

1. Select the check box for the name of this schema\. DMS Schema Conversion highlights the schema name in blue and activates the **Actions** menu\.

1. For **Actions**, choose **Save as SQL**\. The **Save** dialog box appears\.

1. Choose **Save as SQL** to confirm your choice\.

1. Choose **S3 bucket**\. The **Amazon S3** console opens\.

1. Choose **Download** to save your SQL scripts\.

 **To apply the converted code to your target database** 

1. In the target database pane, choose the converted database schema\.

1. Select the check box for the name of this schema\. DMS Schema Conversion highlights the schema name in blue and activates the **Actions** menu\.

1. For **Actions**, choose **Apply changes**\. The **Apply changes** dialog box appears\.

1. Choose **Apply** to confirm your choice\.

Now you have successfully converted your source SQL Server database schemas to MySQL\. To complete the database migration, move your data and connect your applications to the new database\.