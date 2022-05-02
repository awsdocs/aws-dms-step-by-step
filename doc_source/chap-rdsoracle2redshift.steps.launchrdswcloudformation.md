# Step 1: Launch the RDS Instances in a VPC by Using the AWS CloudFormation Template<a name="chap-rdsoracle2redshift.steps.launchrdswcloudformation"></a>

Before you begin, you’ll need to download an AWS CloudFormation template\. Follow these instructions:

1. Download the following archive to your computer: ` [http://docs\.aws\.amazon\.com/dms/latest/sbs/samples/dms\-sbs\-RDSOracle2Redshift\.zip](http://docs.aws.amazon.com/dms/latest/sbs/samples/dms-sbs-RDSOracle2Redshift.zip) ` 

1. Extract the AWS CloudFormation template \(`Oracle_Redshift_For_DMSDemo.template`\) from the archive\.

1. Copy and paste the `Oracle_Redshift_For_DMSDemo.template` file into your current directory\.

Now you need to provision the necessary AWS resources for this walkthrough\.

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create Stack**\.

1. On the **Select Template **page, choose **Upload a template to Amazon S3**\.

1. Click **Choose File**, and then choose the `Oracle_Redshift_For_DMSDemo.template` file that you extracted from the `dms-sbs-RDSOracle2Redshift.zip` archive\.

1. Choose **Next**\. On the **Specify Details** page, provide parameter values as shown following\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-rdsoracle2redshift.steps.launchrdswcloudformation.html)  
![\[Specify Details page\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift3.png)

1. Choose **Next**\. On the **Options** page, choose **Next**\.

1. On the **Review** page, review the details, and if they are correct choose **Create**\.  
![\[replication instance\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift5.png)

1.  AWS can take about 20 minutes or more to create the stack with an Amazon RDS for Oracle instance and an Amazon Redshift cluster\.  
![\[Create Stack page\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-rdsor2redshift6.png)

1. After the stack is created, select the **OracletoRedshiftDWusingDMS** stack, and then choose the **Outputs** view\. Record the JDBC connection strings, **OracleJDBCConnectionString** and **RedshiftJDBCConnectionString**, for use later in this walkthrough to connect to the Oracle and Amazon Redshift databases\.