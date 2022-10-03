# pglogical<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical"></a>

The pglogical extension for PostgreSQL implements logical streaming replication, using a similar publish and subscribe built\-in approach\.

The pglogical extension is suitable for the following use cases if:
+ Your database size is greater than 100 GB\.
+ You want to replicate the schema, DDL, sequences, and table data\.
+ You want to capture ongoing changes\.
+ You want to avoid downtime\.

The pglogical extension may not be suitable for the following use cases if:
+ You have `UNLOGGED` and `TEMPORARY` tables\.
+ You plan to migrate database metadata\.

## Example<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-example"></a>

The following example shows how to migrate the public schema\.

## Configure the Source Database<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-source"></a>

To configure the source database with logical replication, complete the following steps\.

1. In the source database, edit the `postgresql.conf` file to add the following parameters\.

   ```
   wal_level = 'logical'
   max_worker_processes = 10
   max_replication_slots = 10
   max_wal_senders = 10
   shared_preload_libraries = 'pglogical'
   ```

1. Restart the source PostgreSQL instance for these parameters to take effect\.

1. To make sure that you configured parameters on your source database correctly, run the following command\.

   ```
   psql -h <hostname> -p 5432 -U <username> -d <database_name> -c "select name, setting from pg_settings where name in ('wal_level','track_commit_timestamp','max_worker_processes','max_replication_slots','max_wal_senders','shared_preload_libraries');"
   
   -h is the name of source server where you would like to migrate your database.
   -U is the name of the user present on the source server.
   -d is the name of database name present on source already.
   ```

## Configure the Target Database<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-target"></a>

By default, Amazon RDS for PostgreSQL and Aurora PostgreSQL have the `pglogical` extension\. To configure the target DB parameter group, complete the following steps\.

1. To turn on the logical replication in the target database, set the following parameters in the database parameter group\. For more information, see [Working with parameter groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html)\.

   ```
   rds.logical_replication=1
   shared_preload_libraries = 'pglogical'
   ```

1. Reboot your Amazon RDS instance for these parameters to take effect\.

1. To make sure that you configured parameters on your target database correctly, run the following command\.

   ```
   psql -h <hostname> -p 5432 -U <username> -d <database_name> -c "select name, setting from pg_settings where name in ('wal_level','track_commit_timestamp','max_worker_processes','max_replication_slots','max_wal_senders','shared_preload_libraries');"
   
   -h is the name of target server where you would like to migrate your database.
   -U is the name of the user present on the target server.
   -d is the name of database name present on target already.
   ```

## Set Up the Logical Replication<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-replication"></a>

Now, you can configure the logical replication between your self\-managed PostgreSQL databases and Amazon RDS for PostgreSQL or Aurora PostgreSQL\.

1. Download the `pglogical rpm` and install it on your source database\.
   + For PostgreSQL 9\.6, run the following command\.

     ```
     curl https://access.2ndquadrant.com/api/repository/dl/default/release/9.6/rpm | bash
     yum install postgresql96-pglogical
     ```
   + For PostgreSQL 10, run the following command\.

     ```
     curl https://access.2ndquadrant.com/api/repository/dl/default/release/10/rpm | bash
     yum install postgresql10-pglogical
     ```

1. Create the `pglogical` extension on your provider and subscriber\.

   ```
   CREATE EXTENSION pglogical;
   ```

1. Create the publisher node on your source database\.

   ```
   SELECT pglogical.create_node(
       node_name := 'publisher_name',
       dsn := 'host=<publisher_hostname> port=port_number dbname=<database_name>'
   );
   
   -publisher_name: Provide the name of the publication created at source.
   -publisher_hostname: Provide the hostname of the source database.
   -port_number: Provide the port on which the source database is running.
   -database_name: Provide the name of the database where the publication is created.
   ```

1. Add tables in public schema to the default replication set\.

   ```
   SELECT pglogical.replication_set_add_all_tables('default', ARRAY['public']);
   ```

1. Create the subscriber node on target database\.

   ```
   SELECT pglogical.create_node(
       node_name := 'subscriber_name',
       dsn := 'host=<subscriber_hostname> port=port_number dbname=<database_name>'
   );
   
   -subscriber_hostname: Provide the hostname of the target database.
   -port_number: Provide the port on which the target database is running.
   -database_name: Provide the name of the database where the subscription is created.
   -subscriber_name: Provide the name of the subscription created at target.
   ```

1. Create the subscription on the subscriber node\. This subscription starts synchronization and replication processes in background\.

   ```
   SELECT pglogical.create_subscription(
       subscription_name := 'subscriber_name',
       provider_dsn := 'host=<subscriber_hostname> port=port_number dbname=<database_name>'
   );
   
   SELECT pglogical.wait_for_subscription_sync_complete('subscriber_name');
   
   -subscriber_hostname: Provide the hostname of the target database.
   -port_number: Provide the port on which the target database is running.
   -database_name: Provide the name of the database where the subscription is created.
   -subscriber_name: Provide the name of the subscription created at target.
   ```

## Verify that the Data Replication Is Running<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-verify"></a>

Make sure that no active transactions or data changes are happening on your source database\. Then, check the status of your replication by running the following statement on your source database\. Make sure that the WAL locations are the same for the `sent_location`, `write_location`, and `replay_location`\. This indicates that the target database is at the same LSN position as the source database\.

```
SELECT * FROM pg_stat_replication;
```

## Stop the Replication<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-stop"></a>

When the data is in sync between your source and target databases, stop the subscriber on your target database\.

```
select pglogical.alter_subscription_disable('subscriber_name');

-subscriber_name: Provide the name of the subscription created at target.
```

Capture the `confirmed_flush_lsn` value from the replication slot created by the `pglogical` setup\. You can use this value as the start position for the AWS DMS task\.

```
SELECT slot_name, confirmed_flush_lsn from pg_replication_slots where slot_name like 'replication_slot_name';
```

## Drop the Subscription<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pglogical-drop"></a>

To drop the subscription on your target database, run the following command\.

```
select pglogical.drop_subscription('subscriber_name');

-subscriber_name: Provide the name of the subscription created at target.
```