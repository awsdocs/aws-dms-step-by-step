# Prerequisites<a name="bigquery-redshift-prerequisites"></a>

The following prerequisites are also required to complete this walkthrough:
+ Familiarity with the AWS Management Console, AWS SCT, Amazon Redshift, Google Cloud management console, and SQL\.
+ An AWS account with AWS Identity and Access Management \(IAM\) credentials\. Make sure that you can use these credentials to launch Amazon Redshift clusters and create an Amazon S3 bucket in your AWS Region\. For more information, see [Creating an IAM role for your Amazon Redshift cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html#authorizing-redshift-service-creating-an-iam-role) and [Create an IAM user for your Amazon S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/setting-up-s3.html#create-an-iam-user-gsg)\.
+ Basic knowledge of the Amazon Virtual Private Cloud \(Amazon VPC\) service and of security groups\. For information about using Amazon Redshift in a VPC, see [Managing clusters in a VPC](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-vpc.html)\.
+ An understanding of the supported features and limitations on using BigQuery as a source for AWS SCT\. For more information, see [Using BigQuery as a source](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Source.BigQuery.html) in AWS SCT User Guide\.
+ An AWS service profile in AWS SCT with access to the S3 bucket\. For more information, see [Storing service profiles](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_UserInterface.html#CHAP_UserInterface.Profiles) in AWS SCT User Guide\.

For more information about the AWS Schema Conversion Tool, see [https://docs\.aws\.amazon\.com/SchemaConversionTool/latest/userguide/CHAP\_Welcome\.html](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html)\.

For this migration walkthrough, we expect that you are familiar with BigQuery\. You can use your BigQuery project for this migration, or create a new one\. For example, you can use one of the public BigQuery datasets that are available in [Google Cloud Marketplace](https://console.cloud.google.com/marketplace/browse?filter=solution-type:dataset)\.

We recommend that you don’t use your production workloads for the migration in this walkthrough\. After you get familiar with migration tools and AWS services, you can migrate your production workloads\.