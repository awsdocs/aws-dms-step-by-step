# Create a migration task<a name="chap-mariadb2auroramysql.createtask"></a>

We’ve now verified that the replication instance can connect to both the source and target endpoints\. The next step is to create a database migration task\.

1. On the navigation pane, choose **Database Migration Tasks**\.

1. Choose **Create Task**\. Provide the specified values for the following, and then choose **Next**:
   +  **Task identifier** – `maria-mysql` 
   +  **Replication instance** – Choose the replication instance, `mariadb-mysql`\.
   +  **Source database endpoint** – Choose the source database, `maria-on-prem`\.
   +  **Target database endpoint** – Choose the target database, `mysqltrg-rds`\.
   +  **Migration Type** – Choose **Migrate existing data and replicate ongoing changes** for CDC, or **Migrate existing data** for full load\.

1. For **Task settings**, choose the following settings:
   +  **Target table preparation mode** – Do nothing
   +  **Stop task after full load completes** – Don’t stop
   +  **Include LOB columns in replication** – Limited LOB mode
   +  **Maximum LOB size \(KB\)** – 32
   +  **Enable validation** 
   +  **Enable CloudWatch logs** 

1. For **Table mappings**, choose the following settings:
   + Schema – Choose **migration** \(assuming the schema and database to be migrated appear correctly\)\.
   + Table name – Enter the table name, or `%` to specify all the tables in the database\.
   + Action – Enter **Include** to include specific tables, or **Exclude** to exclude specific tables\.

1. Choose **Create Task**\.

Your new AWS DMS migration task reads the data from the tables in the MariaDB source and migrates your data to the Aurora MySQL target\.