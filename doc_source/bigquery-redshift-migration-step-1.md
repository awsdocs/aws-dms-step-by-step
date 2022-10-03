# Step 1: Create a BigQuery Service Account Key File<a name="bigquery-redshift-migration-step-1"></a>

You can connect to BigQuery with a user account or a service account\. A *service account* is a special kind of account designed to be used by applications or compute workloads, rather than a person\.

Service accounts don’t have passwords and use a unique email address for identification\. You can associate each service account with a service account key, which is a public or private RSA key pair\. In this walkthrough, we use a service account key in AWS SCT to access your BigQuery project\.

 **To create a BigQuery service account key** 

1. Sign in to the [Google Cloud management console](https://console.cloud.google.com/)\.

1. Make sure that you have API enabled on your [BigQuery API](https://console.cloud.google.com/apis/library/bigquery.googleapis.com) page\. If you don’t see **API Enabled**, choose **Enable**\.

1. On the [Service accounts](https://console.cloud.google.com/iam-admin/serviceaccounts) page, choose your BigQuery project, and then choose **Create service account**\.

1. On the **Service account details** page, enter a descriptive value for **Service account name**\. Choose **Create and continue**\. The **Grant this service account access to the project** page opens\.

1. For **Select a role**, choose **BigQuery**, and then choose **BigQuery Admin**\. AWS SCT uses permissions to manage all resources within the project to load your BigQuery metadata in the migration project\.

1. Choose **Add another role**\. For **Select a role**, choose **Cloud Storage**, and then choose **Storage Admin**\. AWS SCT uses full control of data objects and buckets to extract your data from BigQuery and then load it into Amazon Redshift\.

1. Choose **Continue**, and then choose **Done**\.

1. On the [Service accounts](https://console.cloud.google.com/iam-admin/serviceaccounts) page, choose the service account that you created\.

1. Choose **Keys**, **Add key**, **Create new key**\.

1. Choose **JSON**, and then choose **Create**\. Choose the folder to save your private key or check the default folder for downloads in your browser\.