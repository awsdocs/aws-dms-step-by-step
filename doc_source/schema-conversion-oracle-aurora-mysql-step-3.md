# Step 3: Create Your Target Aurora MySQL Database<a name="schema-conversion-oracle-aurora-mysql-step-3"></a>

In this step, you create a new Aurora MySQL database to use as a migration target for DMS Schema Conversion\. Also, you configure a new database user on your target Aurora MySQL database\.

If you already created the target database, skip this step and proceed with the configuration of your database user\.

 **To create an Aurora MySQL database for DMS Schema Conversion** 

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Choose your AWS Region\.

1. Choose **Create database**\.

1. For **Engine type**, choose **Amazon Aurora**\.

1. For **Edition**, choose **Amazon Aurora MySQL\-Compatible Edition**\.

1. For **Templates**, choose **Dev/Test**\.

1. For **DB cluster identifier**, enter a unique name for your MySQL database\.

1. For **Master password** and **Confirm master password**, enter a secure password that includes at least 8 printable characters\.

1. For **Virtual private cloud \(VPC\)** under **Connectivity**, choose `sc-vpc`\. You created this VPC in [Step 1](schema-conversion-oracle-aurora-mysql-step-1.md)\.

1. For **Public access**, choose **Yes**\.

1. Keep the rest of the settings as they are, and then choose **Create database**\.

After you create your Aurora MySQL database, configure a new database user\. Then, use the credentials of this user in DMS Schema Conversion\. We encourage not using the admin user in the DMS Schema Conversion migration project\.

To configure your target database user, create a new user and grant the following privileges:
+ CREATE ON **\.** 
+ ALTER ON **\.** 
+ DROP ON **\.** 
+ INDEX ON **\.** 
+ REFERENCES ON **\.** 
+ SELECT ON **\.** 
+ CREATE VIEW ON **\.** 
+ SHOW VIEW ON **\.** 
+ TRIGGER ON **\.** 
+ CREATE ROUTINE ON **\.** 
+ ALTER ROUTINE ON **\.** 
+ EXECUTE ON **\.** 
+ CREATE TEMPORARY TABLES ON **\.** 
+ INVOKE LAMBDA ON **\.** 
+ INSERT, UPDATE ON AWS\_ORACLE\_EXT\.\*
+ INSERT, UPDATE, DELETE ON AWS\_ORACLE\_EXT\_DATA\.\*
+ CREATE TEMPORARY TABLES ON AWS\_ORACLE\_EXT\_DATA\.\*

You can use the following code example to create a database user and grant the privileges\.

```
CREATE USER 'user_name' IDENTIFIED BY 'your_password';
GRANT CREATE ON *.* TO 'user_name';
GRANT ALTER ON *.* TO 'user_name';
GRANT DROP ON *.* TO 'user_name';
GRANT INDEX ON *.* TO 'user_name';
GRANT REFERENCES ON *.* TO 'user_name';
GRANT SELECT ON *.* TO 'user_name';
GRANT CREATE VIEW ON *.* TO 'user_name';
GRANT SHOW VIEW ON *.* TO 'user_name';
GRANT TRIGGER ON *.* TO 'user_name';
GRANT CREATE ROUTINE ON *.* TO 'user_name';
GRANT ALTER ROUTINE ON *.* TO 'user_name';
GRANT EXECUTE ON *.* TO 'user_name';
GRANT INSERT, UPDATE ON AWS_ORACLE_EXT.* TO 'user_name';
GRANT INSERT, UPDATE, DELETE ON AWS_ORACLE_EXT_DATA.* TO 'user_name';
GRANT CREATE TEMPORARY TABLES ON AWS_ORACLE_EXT_DATA.* TO 'user_name';
```

In the preceding example, replace *user\_name* with the name of your user\. Then, replace *your\_password* with a secure password\.

In your target Amazon Aurora MySQL database, set the `lower_case_table_names` parameter to `1`\. Also, set the `log_bin_trust_function_creators` parameter to `1`, and the `character_set_server` parameter to `latin1`\.