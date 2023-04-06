# Step 2: Configure Your Source Database<a name="schema-conversion-sql-server-aurora-postgresql-step-2"></a>

In this step, you configure a new database user on your source SQL Server database\. Also, you configure the network to set up interaction for your source database with DMS Schema Conversion\.

Use the credentials of this new user in DMS Schema Conversion\. We encourage not using the admin user in the DMS Schema Conversion migration project\.

Make sure that you grant the following privileges to this new user to complete the migration:
+  `VIEW DEFINITION` — makes it possible for users that have public access to see object definitions\.
+  `VIEW DATABASE STATE` — makes it possible for users to check the features of the SQL Server Enterprise edition\.

You can use the following code example to create a database user and grant the privileges\.

```
USE db_name
CREATE USER user_name FOR LOGIN user_name
GRANT VIEW DEFINITION TO user_name
GRANT VIEW DATABASE STATE TO user_name
```

In the preceding example, replace *user\_name* with the name of your user\. Then, replace *db\_name* with a name of your database\.

Repeat the grant for each database whose schema you are converting\.

Then, make sure that you grant the following privileges on the `master` database:
+  `VIEW SERVER STATE` — makes it possible for users to collect server settings and configuration\.
+  `VIEW ANY DEFINITION` — makes it possible for users to view data providers\.

You can use the following code example to create a database user and grant the privileges\.

```
USE master
GRANT VIEW SERVER STATE TO user_name
GRANT VIEW ANY DEFINITION TO user_name
```

In the preceding example, replace *user\_name* with the name of your user\.

After you configure your database user, make sure that DMS Schema Conversion can access your source SQL Server database\. To set up a network for DMS Schema Conversion, you can use different network configurations\. These configurations depend on the settings of your source database and your network\. For more information about available options, see [Setting up a network for DMS Schema Conversion](https://docs.aws.amazon.com/dms/latest/userguide/instance-profiles-network.html)\.

In this walkthrough, you configure a Site\-to\-Site VPN connection using a virtual private gateway\.

 **To configure a Site\-to\-Site VPN connection** 

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Choose your AWS Region\.

1. Create a customer gateway\.
   + In the navigation pane, choose **Customer gateways**, and then **Create customer gateway**\.
   + For **Name tag**, enter a name for your customer gateway\.
   + For **BGP ASN**, enter a Border Gateway Protocol \(BGP\) Autonomous System Number \(ASN\) for your customer gateway\.
   + For **IP address**, enter the static, internet\-routable IP address for your customer gateway device\.
   + For **Certificate ARN**, choose the Amazon Resource Name of the private certificate\.
   + For **Device**, enter a name for the device that hosts this customer gateway\.

1. Create a virtual private gateway\.
   + In the navigation pane, choose **Virtual private gateways**, and then **Create virtual private gateway**\.
   + For **Name tag**, enter a name for your virtual private gateway\.
   + For **Autonomous System Number \(ASN\)**, choose **Amazon default ASN**\.
   + Choose **Create virtual private gateway**\.
   + Select the virtual private gateway you created, choose **Actions**, and then **Attach to VPC**\.
   + Under **Available VPCs**, select your VPC from the list and choose **Attach to VPC**\.

1. Configure route propagation in your route table\.
   + In the navigation pane, choose **Route tables**, and then select the route table that is associated with your subnet\. By default, this is the main route table for the VPC\.
   + On the **Route propagation** tab in the details pane, choose **Edit route propagation**\.
   + Select the virtual private gateway that you created before, and then choose **Save**\.

1. Add rules to your security group\.
   + In the navigation pane, choose **Security groups**, and then select the default security group for your VPC\.
   + On the **Inbound** tab in the details pane, add rules that allow inbound SSH, RDP, and ICMP access from your network\.
   + Choose **Save**\.

1. Create a Site\-to\-Site VPN connection\.
   + In the navigation pane, choose **Site\-to\-Site VPN connections**, and then **Create VPN connection**\.
   + For **Name tag**, enter a name for your Site\-to\-Site VPN connection\.
   + For **Target gateway type**, choose either **Virtual private gateway**\.
   + For **Customer gateway**, select **Existing**\.
   + For **Customer gateway ID**, choose the customer gateway that you created before\.
   + Select the routing option\. If your customer gateway device supports BGP, then choose **Dynamic \(requires BGP\)**\. Alternatively, choose **Static** and specify IP prefixes for the private network of your Site\-to\-Site VPN connection\.
   + For **Outside IP address type**, keep the default option\.
   + Choose **Create VPN connection**\.

1. Download the configuration file\.
   + In the navigation pane, choose **Site\-to\-Site VPN connections**, and then **Download configuration**\.
   + Select the `vendor`, `platform`, `software`, and `IKE version` that correspond to your customer gateway device\. If your device isn’t listed, choose `Generic`\.
   + Choose **Download**\.

1. Use the sample configuration file to configure your customer gateway device\.