# Step 4: Create a Target Amazon S3 Bucket<a name="oracle-s3-data-lake-step-4"></a>

Before you create the target endpoint, you create an Amazon S3 bucket for the target data lake\.

To create the Amazon S3 bucket, do the following:

1. Sign in to the AWS Management Console, and open the [Amazon S3 console](https://s3.console.aws.amazon.com/s3/home)\.

1. Choose **Create bucket**\.

1. For **Bucket name**, enter `s3-datalake`\.

1. For ** AWS Region**, choose the region that hosts your AWS DMS replication instance\.

1. Keep the default values in the other fields and choose **Create bucket**\.

You can also plan to optimize the storage cost from Amazon S3 using [Intelligent\-Tiering](https://aws.amazon.com/s3/storage-classes/intelligent-tiering/) and [Lifecycle policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) when storing huge volume of data\.

Now, you have the Amazon S3 bucket for your data lake\. Next, you can create a target endpoint for this bucket\.