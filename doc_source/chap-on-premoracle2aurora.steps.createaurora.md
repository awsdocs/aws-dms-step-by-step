# Step 5: Create Your Aurora MySQL Target Endpoint<a name="chap-on-premoracle2aurora.steps.createaurora"></a>

Next, you can provide information for the target Amazon Aurora MySQL database by specifying the target endpoint settings\.

To specify a target database endpoint, do the following:

1. In the AWS DMS console, choose **Endpoints** on the navigation pane\.

1. Choose **Create endpoint**\. The **Create endpoint** appears, as shown following\.

1. Specify your connection information for the target Aurora MySQL database\. The following table describes the target settings\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dms/latest/sbs/chap-on-premoracle2aurora.steps.createaurora.html)

1. Define additional specific settings for your endpoints using wizard or editor in **Endpoint settings**\.

1. Choose the encryption key to use to encrypt replication storage and connection information in **KMS key**\. If you choose **\(Default\) aws/dms**, the default AWS Key Management Service \(AWS KMS\) key associated with your user and region is used\.

1. Add tags to organize your DMS resources in **Tags**\. You can use tags to manage your IAM roles and policies, and track your DMS costs\.

Prior to saving your endpoint, you have an opportunity to test it in **Test endpoint connection \(optional\)**\. To do so you’ll need to choose a VPC and replication instance from which to perform the test\.