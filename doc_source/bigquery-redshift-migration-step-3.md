# Step 3: Create Buckets to Store Your Temporary Data<a name="bigquery-redshift-migration-step-3"></a>

Data migration from BigQuery to Amazon Redshift includes the following steps:

1. Export data from BigQuery to a Cloud Storage bucket\.

1. Extract data from a Cloud Storage bucket\.

1. Upload data to an Amazon Simple Storage Service \(Amazon S3\) bucket\.

1. Copy data from an S3 bucket to Amazon Redshift\.

You need all four steps because you can’t access data directly in BigQuery and you can’t upload data directly to Amazon Redshift\. Because of these limitations, you need to create buckets to store your data during migration\.

 **To create a Cloud Storage bucket** 

1. Sign in to the [Google Cloud management console](https://console.cloud.google.com/)\.

1. Open the [Cloud Storage Browser](https://console.cloud.google.com/storage/browser) page\.

1. Choose **Create bucket**\.

1. For **Name your bucket**, enter a name for your Cloud Storage bucket\.

1. On the **Choose where to store your data** page, choose **Region** for **Location type** and then choose your region for **Location**\.

1. Leave the default values for other options, and choose **Create**\.

 **To create an Amazon S3 bucket** 

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [ https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\.

1. For **Bucket name**, enter the globally unique name of your Amazon S3 bucket\.

1. For ** AWS Region**, choose the AWS Region where you want the bucket to reside\. Choose a Region close to you to minimize latency and costs\.

1. Leave the default values for other options, and choose **Create bucket**\.