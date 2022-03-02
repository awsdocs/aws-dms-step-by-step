# Step 4: Configure a Target Amazon S3 Bucket<a name="chap-rdssqlserver2s3datalake.steps.targets3bucket"></a>

You can integrate Amazon S3 with other AWS and third\-party services to take advantage of the following:
+ Data analysis using Amazon Athena query engine\. This service helps reduce cost as you do not pay for dedicated resources and instead pay based on the amount data being scanned\.
+ Perform extract, transform, and load \(ETL\) operations using distributed processing frameworks such as Spark with Amazon EMR or AWS Glue\.
+ Implement machine learning use cases, because Amazon S3 can store granular time series data spanning years in raw form, in conjunction with Amazon SageMaker\.

Because in this use case we migrate the `Sales` schema to Amazon S3, we need to account for future use cases of the migrated data before we set up Amazon S3 bucket and AWS DMS endpoints\.

To create the Amazon S3 bucket, do the following:

1. Open the Amazon S3 console at [https://s3\.console\.aws\.amazon\.com/s3/home](https://s3.console.aws.amazon.com/s3/home)\.

1. Choose **Create bucket**\.

1. For **Bucket name**, enter **adventure\-works\-datalake**\.

1. For **AWS Region**, choose the region that hosts your AWS DMS replication instance\.

1. Leave the default values in the other fields and choose **Create bucket**\.