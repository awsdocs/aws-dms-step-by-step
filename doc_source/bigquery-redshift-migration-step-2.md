# Step 2: Create an Amazon Redshift Cluster<a name="bigquery-redshift-migration-step-2"></a>

To store your data in the AWS cloud, you can use your existing Amazon Redshift cluster or create a new one\. You don’t need to create any tables because AWS SCT automates this process\.

If you don’t plan to migrate data as part of this walkthrough, you can skip this step\. To see how AWS SCT converts your database code objects, use a virtual Amazon Redshift target in your project\. For more information, see [Using virtual targets](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Mapping.VirtualTargets.html)\.

 **To create an Amazon Redshift cluster** 

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\.

1. Choose **Create cluster**\.

1. For **Cluster identifier**, enter the unique name of your Amazon Redshift cluster\.

1. Choose **Free trial**\.

1. For **Admin user name**, enter the login for the admin user of your Amazon Redshift cluster\.

1. For **Admin user password**, enter the password for the admin user\.

1. Choose **Create cluster**\.

After you create your Amazon Redshift database, configure a new database user\. Then, use the credentials of this user in AWS SCT to access your Amazon Redshift cluster\. We don’t recommend you to use the admin user for the migration\.

Make sure that you grant the following privileges to this new user to complete the migration:
+  `CREATE ON DATABASE` — allows to create new schemas in the database\.
+  `GRANT USAGE ON LANGUAGE` — allows to create new functions and procedures in the database\.
+  `GRANT SELECT ON ALL TABLES IN SCHEMA pg_catalog` — provides the user with system information about the Amazon Redshift cluster\.
+  `GRANT SELECT ON pg_class_info` — provides the user with information about tables distribution style\.

You can use the following code example to create a database user and grant the privileges\.

```
CREATE USER user_name PASSWORD your_password;
GRANT CREATE ON DATABASE db_name TO user_name;
GRANT USAGE ON LANGUAGE plpythonu TO user_name;
GRANT USAGE ON LANGUAGE plpgsql TO user_name;
GRANT SELECT ON ALL TABLES IN SCHEMA pg_catalog TO user_name;
GRANT SELECT ON pg_class_info TO user_name;
GRANT SELECT ON sys_serverless_usage TO user_name;
GRANT SELECT ON pg_database_info TO user_name;
GRANT SELECT ON pg_statistic TO user_name;
```

In the example preceding, replace `user_name` with the name of your user\. Then, replace `db_name` with the name of your target Amazon Redshift database\. Finally, replace `your_password` with a secure password\.