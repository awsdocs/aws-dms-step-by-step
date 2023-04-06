# Prerequisites<a name="schema-conversion-oracle-aurora-mysql-prerequisites"></a>

The following prerequisites are also required to complete this walkthrough:
+ Familiarity with the AWS Management Console, AWS Database Migration Service, and SQL\.
+ A user with AWS Identity and Access Management \(IAM\) credentials\. Make sure that you can use these credentials to create an Amazon S3 bucket in your AWS Region\.
+ Basic knowledge of the Amazon Virtual Private Cloud \(Amazon VPC\) service and of security groups\.
+ An understanding of the supported features and limitations of DMS Schema Conversion\. For more information, see [Schema conversion limitations](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SchemaConversion.html#schema-conversion-limitations)\.

We recommend that you don’t use your production workloads for the migration in this walkthrough\. After you get familiar with migration tools and AWS services, you can migrate your production workloads\.

Make sure that you create all your AWS and DMS Schema Conversion resources in the AWS Regions that support DMS Schema Conversion\. For more information, see the [list of supported Regions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SchemaConversion.html#schema-conversion-supported-regions)\. In other Regions, you can use the AWS Schema Conversion Tool \(AWS SCT\)\. To download AWS SCT, see [Installing, verifying, and updating](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Installing.html) in the *Schema Conversion Tool User Guide*\.

For more information about DMS Schema Conversion, see the [user guide](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SchemaConversion.html)\.