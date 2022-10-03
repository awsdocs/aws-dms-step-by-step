# Publisher and Subscriber<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher"></a>

In PostgreSQL, logical replication uses a publisher and subscriber model\. In this model, one or more subscribers subscribe to one or more publications on a publisher node\. Subscribers pull data from the publications they subscribe to and may subsequently re\-publish data to allow cascading replication or more complex configurations\. PostgreSQL version 10\.0 and higher supports the native publisher and subscriber model\.

The publisher and subscriber model is suitable for the following use cases if:
+ Your database size is greater than 100 GB\.
+ You have an existing schema in your target database and migrate only data\.
+ You want to capture ongoing changes\.
+ You want to minimize downtime\.

The publisher and subscriber may not be suitable for the following use cases if:
+ You plan to migrate database metadata\.
+ Your source PostgreSQL database version is lower than 10\.x\.
+ You want to replicate the schema, DDL, and sequences\.

## Example<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher-example"></a>

The migration process involves copying a snapshot of the publishing database to the subscriber\. This step is also called the table synchronization phase\. To reduce the time spent in this phase, you can spawn multiple table synchronization workers\. However, you can only have one synchronization worker for each table\.

The following example shows how to migrate all tables from a public schema\.

## Configure the Source Database<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher-configure"></a>

To configure the source database with built\-in logical replication, complete the following steps\.

1. In the source database, edit the `postgresql.conf` file to add the following parameters\.

   ```
   wal_level = 'logical'
   max_replication_slots = 10
   max_wal_senders = 10
   ```

   Make sure that you set `wal_level` to `logical`\. This adds information necessary to support logical decoding\. Also, make sure that you set `max_replication_slots` to at least the number of subscriptions expected to connect, plus some reserve for table synchronization\. Finally, make sure that you set `max_wal_senders` to at least the same value as `max_replication_slots` plus the number of physical replicas that are connected at the same time\.

1. Restart the source PostgreSQL instance for these parameters to take effect\.

1. To make sure that you configured parameters on your source database correctly, run the following command\.

   ```
   psql -h <hostname> -p 5432 -U <username> -d <database_name> -c "select name, setting from pg_settings where name in ('wal_level','max_worker_processes','max_replication_slots','max_wal_senders');"
   
   -h is the name of source server where you would like to migrate your database.
   -U is the name of the user present on the source server.
   -d is the name of database name present on source already.
   ```

## Set Up the Logical Replication<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher-replication"></a>

Now, you can configure built\-in logical replication between your self\-managed PostgreSQL databases and Amazon RDS for PostgreSQL or Aurora PostgreSQL\.

To create the publication on the source database server, run the following command\.

```
CREATE PUBLICATION my_publication FOR ALL TABLES;
```

You can specify only the tables that you want to publish\. You can also limit the changes that will be published\.

To replicate `DELETE` and `UPDATE` operations, make sure that the `published` table has a replica identity, which can be a primary key\. This makes it possible for the subscriber to identify the modified rows\. You can replicate `INSERT` operations without a replica identity\. After you created the publications, you can create subscriptions in the subscriber node\.

Before you create a subscriber node, make sure that you created the target Amazon RDS for PostgreSQL or Aurora PostgreSQL database\. For more information, see [Creating a PostgreSQL DB instance and connecting to a database on a PostgreSQL DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.PostgreSQL.html)\.

To create a database on Amazon RDS for PostgreSQL or Aurora PostgreSQL, run the following command\.

```
psql -h <hostname> -p 5432 -U <username> -d <database_name> -c "create database migrated_database;"

-h is the name of target server where you would like to migrate your database.
-U is the name of the user present on the target server.
-d is the name of database name present on target already.
```

To create the subscription on your target database, run the following command\.

```
CREATE SUBSCRIPTION <subsription_name>
CONNECTION 'host=<host> port=<port_number> dbname=<database_name> user=<username> password=<password>' PUBLICATION <publication_name> WITH copy_data=true;

-subscription_name: Provide the name of the subscription created at target.
-host: Provide the hostname of the source database.
-port_number: Provide the port on which the source database is running.
-database_name: Provide the name of the database where the publication is created.
-publication_name: Provide the name of the publication created at source.
-copy_data: Specifies whether the existing data in the publications that are being subscribed to should be copied once the replication starts. The default is true.
```

## Verify that the Data Replication Is Running<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher-verify"></a>

Make sure that no active transactions or data changes are happening on your source database\. Then, check the status of your replication by running the following statement on your source database\. Make sure that the WAL locations are the same for the `sent_location`, `write_location`, and `replay_location`\. This indicates that the target database is at the same LSN position as the source database\.

```
SELECT * FROM pg_stat_replication;
```

## Stop the Replication<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher-stop"></a>

When the data is in sync between your source and target databases, stop the subscriber on your target database\.

```
ALTER SUBSCRIPTION <subsription_name> DISABLE;

-subscription_name: Provide the name of the subscription created at target.
```

Capture the slot name created by the publisher and subscriber on the target database\.

```
select subslotname from pg_subscription where subname like 'subsription_name';

-subscription_name: Provide the name of the subscription created at target.
```

Capture the `confirmed_flush_lsn` value from the replication slot fetched\. You can use this value as the start position for the AWS DMS task\.

```
SELECT slot_name, confirmed_flush_lsn from pg_replication_slots where slot_name like 'replication_slot_name';
```

## Drop Publication and Subscription Artifacts<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-publisher-drop"></a>

To drop the subscription on your target database, run the following command\.

```
DROP SUBSCRIPTION <subsription_name>;

-subscription_name: Provide the name of the subscription created at target.
```

To drop the publication on your source database, run the following command\.

```
DROP PUBLICATION <publication_name>;

-publication_name: Provide the name of the publication created at source.
```