# Step 1: Create AWS Resources<a name="schema-conversion-oracle-aurora-mysql-step-1"></a>

In this step, you create and configure the required AWS resources for DMS Schema Conversion\.

First, you create a virtual private cloud \(VPC\)\. This VPC is based on the Amazon Virtual Private Cloud \(Amazon VPC\) service and contains your AWS resources\. Make sure that you create this VPC in one of the AWS Regions that support DMS Schema Conversion\. For more information, see the [list of supported Regions](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_SchemaConversion.html#schema-conversion-supported-regions)\.

 **To create a VPC for DMS Schema Conversion** 

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Choose your AWS Region\.

1. Choose **Create VPC**\.

1. On the **Create VPC** page, enter the following settings:
   +  **Resources to create** — **VPC and more** 
   +  **Name tag auto\-generation** — Choose **Auto\-generate** and enter a globally unique name\. For example, enter `sc-vpc`\.
   +  **IPv4 CIDR block** — `10.0.1.0/24` 
   +  **NAT gateways** — **In 1 AZ** 
   +  **VPC endpoints** — **None** 

1. Keep the rest of the settings as they are, and then choose **Create VPC**\.

1. Choose **Subnets**\.
   + For **Filter by VPC**, choose **sc\-vpc**\.
   + Take a note of your two private subnet IDs\. Private subnet IDs don’t include `Public` in the name\.

1. Choose **NAT gateways**\.
   + Choose your **NAT gateway**\.
   + Take a note of your **Elastic IP** address\.

Use this VPC when you create your instance profile in [Step 5](schema-conversion-oracle-aurora-mysql-step-5.md) and your target Aurora database in [Step 3](schema-conversion-oracle-aurora-mysql-step-3.md)\.

Next, you create AWS Identity and Access Management \(IAM\) roles to use in your DMS Schema Conversion migration project\. AWS DMS uses these IAM roles to access your Amazon S3 bucket and database credentials stored in AWS Secrets Manager\.

 **To create an IAM role that provides access to your Amazon S3 bucket** 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. On the **Select trusted entity** page, choose ** AWS service**\. Choose **DMS**\.

1. Choose **Next**\. The **Add permissions** page opens\.

1. For **Filter policies**, enter `S3`\. Choose **AmazonS3FullAccess**\.

1. Choose **Next**\. The **Name, review, and create** page opens\.

1. For **Role name**, enter a descriptive name\. For example, enter `sc-s3-role`\. Choose **Create role**\.

Use this IAM role when you create your instance profile in [Step 5](schema-conversion-oracle-aurora-mysql-step-5.md)\.

 **To create an IAM role that provides access to AWS Secrets Manager ** 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. On the **Select trusted entity** page, choose ** AWS service**\. Choose **DMS**\.

1. Choose **Next**\. The **Add permissions** page opens\.

1. For **Filter policies**, enter `Secret`\. Choose **SecretsManagerReadWrite**\.

1. Choose **Next**\. The **Name, review, and create** page opens\.

1. For **Role name**, enter a descriptive name\. For example, enter `sc-secrets-manager-role`\. Choose **Create role**\.

1. On the **Roles** page, enter `sc-secrets-manager-role` for **Role name**\. Choose **sc\-secrets\-manager\-role**\.

1. On the **sc\-secrets\-manager\-role** page, choose the **Trust relationships** tab\. Choose **Edit trust policy**\.

1. On the **Edit trust policy** page, edit the trust relationships for the role to use your AWS DMS regional service principal as the trusted entity\. This principal has the following format\.

   ```
   dms.region-name.amazonaws.com
   ```

   Replace *region\-name* the name of your Region, such as `us-east-1`\.

   The following code example shows the principal for the `us-east-1` Region\.

   ```
   dms.us-east-1.amazonaws.com
   ```

1. Choose **Update trust policy**\.

Use this IAM role when you create your migration project in [Step 7](schema-conversion-oracle-aurora-mysql-step-7.md)\.

Next, you create an Amazon S3 bucket to use in your DMS Schema Conversion migration project\. DMS Schema Conversion uses this Amazon S3 bucket to save assessment reports, SQL scripts with the converted code, and database metadata\.

 **To create an Amazon S3 bucket for DMS Schema Conversion** 

1. Sign in to the AWS Management Console and open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\.

1. On the **Create bucket** page, select a globally unique name for your S3 bucket\. For example, enter `sc-s3-bucket`\.

1. For ** AWS Region**, choose your Region\.

1. For **Bucket Versioning**, choose **Enable**\.

1. Keep the rest of the settings as they are, and then choose **Create bucket**\.

Use this Amazon S3 bucket when you create your instance profile in [Step 5](schema-conversion-oracle-aurora-mysql-step-5.md)\.