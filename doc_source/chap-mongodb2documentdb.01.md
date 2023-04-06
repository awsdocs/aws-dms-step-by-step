# Launch an Amazon EC2 instance<a name="chap-mongodb2documentdb.01"></a>

For this tutorial, you launch an Amazon EC2 instance into your default VPC\.

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Choose **Launch Instance**, and do the following:

   1. On the **Choose an Amazon Machine Image \(AMI\)** page, at the top of the list of AMIs, go to **Amazon Linux AMI** and choose **Select**\.

   1. On the **Choose an Instance Type** page, at the top of the list of instance types, choose **t2\.micro**\. Then choose **Next: Configure Instance Details**\.

   1. On the **Configure Instance Details** page, for **Network**, choose your default VPC\. Then choose **Next: Add Storage**\.

   1. On the **Add Storage** page, skip this step by choosing **Next: Add Tags**\.

   1. On the **Add Tags** page, skip this step by choosing **Next: Configure Security Group**\.

   1. On the **Configure Security Group** page, do the following:

      1. Choose **Select an existing security group**\.

      1. In the list of security groups, choose **default**\. Doing this chooses the default security group for your VPC\. By default, the security group accepts inbound Secure Shell \(SSH\) connections on TPC port 22\. If this isn’t the case for your VPC, add this rule; for more information, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide*\.

      1. Choose **Next: Review and Launch**\.

   1. Review the information, and choose **Launch**\.

1. In the **Select an existing key pair or create a new key pair** window, do one of the following:
   + If you don’t have an Amazon EC2 key pair, choose **Create a new key pair** and follow the instructions\. You are asked to download a private key file \(\.pem file\)\. You need this file later when you log in to your Amazon EC2 instance\.
   + If you already have an Amazon EC2 key pair, for **Select a key pair** choose your key pair from the list\. You must already have the private key file \(\.pem file\) available in order to log in to your Amazon EC2 instance\.

1. After you configure your key pair, choose **Launch Instances**\.

   In the console navigation pane, choose **EC2 Dashboard**, and then choose the instance that you launched\. In the lower pane, on the **Description** tab, find the **Public DNS** location for your instance, for example: `ec2-11-22-33-44.us-west-2.compute.amazonaws.com`\.

   It takes a few minutes for your Amazon EC2 instance to become available\.

1. Use the `ssh` command to log in to your Amazon EC2 instance, as in the following example\.

   ```
   chmod 400 my-keypair.pem
   ssh -i my-keypair.pem ec2-user@public-dns-name
   ```

   Specify your private key file \(\.pem file\) and the public DNS name of your EC2 instance\. The login ID is `ec2-user`\. No password is required\.

   For further details about connecting to your EC instance, see [Connecting to your Linux instance using SSH](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html) in the *Amazon EC2 User Guide for Linux Instances*\.