# Create source and target endpoints<a name="chap-mongodb2documentdb.04"></a>

The source endpoint is the endpoint for your MongoDB installation running on your Amazon EC2 instance\.

1. Open the AWS DMS console at [https://console\.aws\.amazon\.com/dms/](https://console.aws.amazon.com/dms/)\.

1. In the navigation pane, choose **Endpoints**\.

1. Choose **Create endpoint** and enter the following information:
   + For **Endpoint type**, choose **Source**\.
   + For **Endpoint identifier**, enter a name that’s easy to remember, for example `mongodb-source`\.
   + For **Source engine**, choose **mongodb**\.
   + For **Server name**, enter the public DNS name of your Amazon EC2 instance, for example `ec2-11-22-33-44.us-west-2.compute.amazonaws.com`\.
   + For **Port**, enter `27017`\.
   + For **SSL mode**, choose **none**\.
   + For **Authentication mode**, choose **none**\.
   + For **Database name**, enter `zips-db`\.
   + For **Authentication mechanism**, choose **default**\.
   + For **Metadata mode**, choose **document**\.

   When the settings are as you want them, choose **Create endpoint**\.

Next, you create a target endpoint\. This endpoint is for your Amazon DocumentDB cluster, which should already be running\. For more information on launching your Amazon DocumentDB cluster, see [Getting started](https://docs.aws.amazon.com/documentdb/latest/developerguide/getting-started.html) in the *Amazon DocumentDB Developer Guide\.* 

**Important**  
Before you proceed, do the following:  
Have available the master user name and password for your Amazon DocumentDB cluster\.
Have available the DNS name and port number of your Amazon DocumentDB cluster, so that AWS DMS can connect to it\. To determine this information, use the following AWS CLI command, replacing ` cluster-id ` with the name of your Amazon DocumentDB cluster\.  

  ```
  aws docdb describe-db-clusters \
      --db-cluster-identifier cluster-id \
      --query "DBClusters[*].[Endpoint,Port]"
  ```
Download a certificate bundle that Amazon DocumentDB can use to verify SSL connections\. To do this, enter the following command\. Here, ` aws-api-domain ` completes the Amazon S3 domain in your AWS Region required to access the specified S3 bucket and the `rds-combined-ca-bundle.pem` file that it provides\.  

  ```
  wget https://s3.aws-api-domain/rds-downloads/rds-combined-ca-bundle.pem
  ```

To create a target endpoint, do the following:

1. In the navigation pane, choose **Endpoints**\.

1. Choose **Create endpoint** and enter the following information:
   + For **Endpoint type**, choose **Target**\.
   + For **Endpoint identifier**, enter a name that’s easy to remember, for example `docdb-target`\.
   + For **Target engine**, choose **docdb**\.
   + For **Server name**, enter the DNS name of your Amazon DocumentDB cluster\.
   + For **Port**, enter the port number of your Amazon DocumentDB cluster\.
   + For **SSL mode**, choose **verify\-full**\.
   + For **CA certificate**, do one of the following to attach the SSL certificate to your endpoint:
     + If available, choose the existing **rds\-combined\-ca\-bundle** certificate from the **Choose a certificate** drop down\.
     + Choose **Add new CA certificate**\. Then, for **Certificate identifier**, enter `rds-combined-ca-bundle`\. For **Import certificate file**, choose **Choose file** and navigate to the `rds-combined-ca-bundle.pem` file that you previously downloaded\. Select and open the file\. Choose **Import certificate**, then choose **rds\-combined\-ca\-bundle** from the **Choose a certificate** drop down\.
   + For **User name**, enter the master user name of your Amazon DocumentDB cluster\.
   + For **Password**, enter the master password of your Amazon DocumentDB cluster\.
   + For **Database name**, enter `zips-db`\.

   When the settings are as you want them, choose **Create endpoint**\.

Now that you’ve created the source and target endpoints, test them to ensure that they work correctly\. Also, to ensure that AWS DMS can access the database objects at each endpoint, refresh the endpoints' schemas\.

To test an endpoint, do the following:

1. In the navigation pane, choose **Endpoints**\.

1. Choose the source endpoint \(`mongodb-source`\), and then choose **Test connection**\.

1. Choose your replication instance \(`mongodb2docdb`\), and then choose **Run test**\. It takes a few minutes for the test to complete, and for the **Status** to change to **successful**\.

   If the **Status** changes to **failed** instead, review the failure message\. Correct any errors that might be present, and test the endpoint again\.

**Note**  
Repeat this procedure for the target endpoint \(`docdb-target`\)\.

To refresh schemas, do the following:

1. In the navigation pane, choose **Endpoints**\.

1. Choose the source endpoint \(`mongodb-source`\), and then choose **Refresh schemas**\.

1. Choose your replication instance \(`mongodb2docdb`\), and then choose **Refresh schemas**\.

**Note**  
Repeat this procedure for the target endpoint \(`docdb-target`\)\.