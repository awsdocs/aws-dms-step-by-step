# Step 3: Create Your Target Amazon RDS for PostgreSQL Database<a name="schema-conversion-oracle-postgresql-step-3"></a>

In this step, you create a new Amazon RDS for PostgreSQL database to use as a migration target for DMS Schema Conversion\. Also, you configure a new database user on your target Amazon RDS for PostgreSQL database\.

If you already created the target database, skip this step and proceed with the configuration of your database user\.

 **To create an Amazon RDS for PostgreSQL database for DMS Schema Conversion** 

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. Choose your AWS Region\.

1. Choose **Create database**\.

1. For **Engine type**, choose **PostgreSQL**\.

1. For **Templates**, choose **Free tier**\.

1. For **DB instance identifier**, enter a unique name for your PostgreSQL database\.

1. For **Master password** and **Confirm master password**, enter a secure password that includes at least 8 printable characters\.

1. For **Virtual private cloud \(VPC\)** under **Connectivity**, choose `sc-vpc`\. You created this VPC in [Step 1](schema-conversion-oracle-postgresql-step-1.md)\.

1. For **Public access**, choose **Yes**\.

1. Keep the rest of the settings as they are, and then choose **Create database**\.

After you create your Amazon RDS for PostgreSQL database, configure a new database user\. Then, use the credentials of this user in DMS Schema Conversion\. We encourage not using the admin user in the DMS Schema Conversion migration project\.

To configure your target database user, create a new user and grant `CREATE ON DATABASE` and the `rds_superuser` role\.

You can use the following code example to create a database user and grant the privileges\.

```
CREATE ROLE user_name LOGIN PASSWORD 'your_password';
GRANT CREATE ON DATABASE db_name TO user_name;
GRANT rds_superuser TO user_name;
ALTER DATABASE db_name OWNER TO user_name;
```

In the preceding example, replace *user\_name* with the name of your user\. Then, replace *your\_password* with a secure password\. Finally, replace *db\_name* with the name of your target Amazon RDS for PostgreSQL database\.