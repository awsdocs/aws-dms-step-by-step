# Step 5: Create an AWS SCT Project<a name="bigquery-redshift-migration-step-5"></a>

After you configure AWS SCT, create a new migration project\.

1. In AWS SCT, choose **File**, then choose **New Project**\.

1. For **Project name**, enter a descriptive name of your project, and then choose **OK**\.

1. Choose **Add source** to add a source BigQuery data warehouse to your project, then choose **BigQuery**, and choose **Next**\.

1. For **Connection name**, enter a name for your source data warehouse\. AWS SCT displays this name in the tree in the left panel\.

1. For **Key path**, choose **Browse** and then choose the BigQuery service account key file that you created in step 1\.

1. Choose **Connect** to close the dialog box and to connect to your BigQuery data warehouse\.

1. Choose **Add target** to add a target Amazon Redshift database to your project, then choose **Amazon Redshift**, and choose **Next**\.

1. If you store your database credentials in AWS Secrets Manager, choose your secret and then choose **Populate**\. For more information, see [Using Secrets Manager](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_UserInterface.html#CHAP_UserInterface.SecretsManager)\.

   If you don’t use Secrets Manager, enter your database credentials manually\.
   + For **Connection name**, enter a name for your target data warehouse\. AWS SCT displays this name in the tree in the right panel\.
   + For **Server name**, enter the server name of the Amazon Redshift cluster that you created in step 2\. You can copy the server name as **JDBC URL** in the **General information** for your Amazon Redshift cluster\. Remove `jdbc:redshift://` from the URL that you copied\.
   + For **Server port**, enter 5439\.
   + For **User name**, enter the name of the user that you created in step 2\.
   + For **Password**, enter the password for the user that you created in step 2\.

1. Turn off **Use AWS Glue** and choose **Connect**\.

1. In the tree in the left panel, choose your BigQuery dataset\. In the tree in the right panel, choose your target Amazon Redshift database\. Choose **Create mapping**\. You can add multiple mapping rules a single AWS SCT project\. For more information about mapping rules, see [Creating mapping rules](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Mapping.html)\.

1. Choose **Main view**\.