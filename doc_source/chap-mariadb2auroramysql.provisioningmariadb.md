# Set up MariaDB as a source database<a name="chap-mariadb2auroramysql.provisioningmariadb"></a>

To provision MariaDB as a source database, download [the Mariadb\_CF\.yaml template](https://aws-database-blog.s3.amazonaws.com/artifacts/mariadb-to-aurora-mysql-migration/Mariadb_CF.yaml)\. This AWS CloudFormation template creates an Amazon RDS for MariaDB instance with the required parameters\.

1. On the AWS Management Console, under **Services**, choose **CloudFormation**\.

1. Choose **Create Stack**\.

1. For **Specify template**, choose **Upload a template file**\.

1. Select **Choose File**\.

1. Choose the `MariaDB.yaml` file\.

1. Choose **Next**\.

1. On the **Specify Stack Details** page, edit the predefined values as needed, and then choose **Next**:
   +  **Stack name** — Enter a name for the stack\.
   +  **CIDR** — Enter the CIDR IP range to access the instance\.
   +  **DBAllocated Storage** — Enter the database storage size in GB\. The default is 20 GB\.
   +  **DBBackupRetentionPeriod** — The number of days to retain backups\.
   +  **DBInstanceClass** — Enter the instance type of the database server\.
   +  **DBMonitoringInterval** — Interval to publish database logs to Amazon CloudWatch\.
   +  **DBSubnetGroup** — Enter the DB subnet group\.
   +  **MariaDBEngine** — Enter the MariaDB engine version\.
   +  **RDSDBName** — Enter the name of the database\.
   +  **VPCID** — Enter the VPC to launch your DB instance\.

1. On the **Configure stack options** page, for **Tags**, specify any optional tags, and then choose **Next**\.

1. On the **Review** page, select **I acknowledge that AWS CloudFormation might create IAM resources**\.

1. Choose **Create Stack**\.

After the Amazon RDS for MariaDB instance is created, log in to MariaDB and run the following statements to create `webdb_user` — a superuser that connects to a DMS instance for migration, and grant necessary privileges\.

```
CREATE USER 'webdb_user'@'%' IDENTIFIED BY '******';
GRANT ALL ON migrate.* TO 'webdb_user'@'%' with grant option;
grant REPLICATION SLAVE ON *.* TO webdb_user;
grant REPLICATION CLIENT ON *.* TO webdb_user;
```

In this walkthrough, we created a database called *migration* and few sample tables, along with stored procedures, triggers, functions, and so on\.The below query provides the list of tables in *migration* database:

```
MariaDB [(none)]> use migration

Database changed
MariaDB [migration]> show tables;
+---------------------+
| Tables_in_migration |
+---------------------+
| animal_count        |
| animals             |
| contacts            |
| seat_type           |
| sport_location      |
| sport_team          |
| sport_type          |
+---------------------+
7 rows in set (0.000 sec)
```

The following query returns a list of secondary indexes\.

```
 MariaDB [migration]> SELECT DISTINCT TABLE_NAME, INDEX_NAME,NON_UNIQUE
    ->             FROM INFORMATION_SCHEMA.STATISTICS
    ->             WHERE TABLE_SCHEMA = 'migration' and INDEX_NAME <> 'PRIMARY';
+----------------+-------------------+------------+
| TABLE_NAME     | INDEX_NAME        | NON_UNIQUE |
+----------------+-------------------+------------+
| sport_location | city_id_sport_loc |          1 |
| sport_team     | sport_team_u      |          0 |
| sport_team     | home_field_fk     |          1 |
+----------------+-------------------+------------+
3 rows in set (0.000 sec)
```

The following query returns a list of triggers\.

```
MariaDB [migration]> select TRIGGER_SCHEMA,TRIGGER_NAME
    ->             from information_schema.triggers
    ->             where TRIGGER_SCHEMA='migration';
+----------------+-----------------------+
| TRIGGER_SCHEMA | TRIGGER_NAME          |
+----------------+-----------------------+
| migration      | increment_animal      |
| migration      | contacts_after_update |
+----------------+-----------------------+
2 rows in set (0.001 sec)
```

The following query returns a list of procedures and functions\.

```
MariaDB [(none)]> select routine_schema as database_name,
    ->             routine_name,
    ->             routine_type as type,
    ->             data_type as return_type
    ->             from information_schema.routines
    ->      where routine_schema not in ('sys', 'information_schema',
    ->                                   'mysql', 'performance_schema');
+---------------+----------------+-----------+-------------+
| database_name | routine_name   | type      | return_type |
+---------------+----------------+-----------+-------------+
| migration     | CalcValue      | FUNCTION  | int         |
| migration     | loadMLBPlayers | PROCEDURE |             |
| migration     | loadNFLPlayers | PROCEDURE |             |
+---------------+----------------+-----------+-------------+
3 rows in set (0.000 sec)
```

After all the data is loaded, use `mysqldump` to back up the database metadata\. The `mysqldump` utility to dump one or more databases for backup or transfer to another database server\. The dump typically contains SQL statements to create the table, populate it, or both\. You can also use `mysqldump` to generate files in comma\-separated value \(CSV\), other delimited text, or XML format\.

Use the following command exports tables and index definitions:

```
$ mysqldump --no-data --no-create-db --single_transaction -u root -p migration --skip-triggers > mysql_tables_indexes.sql
```

Use following command to exports routines \(stored procedures, functions, and triggers\) into the file `routines.sql`:

```
$ mysqldump -u root --routines --no-create-info --no-data --no-create-db --skip-opt -p migration > routines.sql
```

The `mysqldump` utility doesn’t provide the option to remove a `DEFINER` statement\. Some MySQL clients provide the option to ignore the definer when creating a logical backup, but this isn’t the default behavior\. Use the following command in a UNIX or Linux environment to remove the `DEFINER` from `routines.sql`:

```
$ sed -i -e 's/DEFINER=`root`@`localhost`/DEFINER=`master`@`%`/g' routines.sql
```

We now have a backup of MariaDB, in two `0sql` files \(`mysql_tables_indexes.sql` and `routines.sql`\)\. We will use these files to load the table definition into an Aurora MySQL database\.

After backups are completed into two \.sql files \(`mysql_tables_indexes.sql`, `routines.sql`\), use these files to load the table definition into the Aurora MySQL database\.