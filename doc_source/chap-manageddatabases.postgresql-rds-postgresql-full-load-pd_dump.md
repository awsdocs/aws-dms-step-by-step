# pg\_dump and pg\_restore<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump"></a>

pg\_dump and pg\_restore is a native PostgreSQL client utility\. You can find this utility as part of the database installation\. It produces a set of SQL statements that you can run to reproduce the original database object definitions and table data\.

The pg\_dump and pg\_restore utility is suitable for the following use cases if:
+ Your database size is less than 100 GB\.
+ You plan to migrate database metadata as well as table data\.
+ You have a relatively large number of tables to migrate\.

The pg\_dump and pg\_restore utility may not be suitable for the following use cases if:
+ Your database size is greater than 100 GB\.
+ You want to avoid downtime\.

## Example<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump-example"></a>

At a high level, you can use the following steps to migrate the [https://github.com/aws-samples/aws-database-migration-samples/tree/master/PostgreSQL/sampledb/v1](https://github.com/aws-samples/aws-database-migration-samples/tree/master/PostgreSQL/sampledb/v1) database\.

1. Export data to one or more dump files\.

1. Create a target database\.

1. Import the dump file or files\.

1. \(Optional\) Migrate database roles and users\.

## Export Data<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump-export"></a>

You can use the following command to create dump files for your source database\.

```
pg_dump -h <hostname> -p 5432 -U <username> -Fc -b -v -f <dumpfilelocation.sql> -d  <database_name>

-h is the name of source server where you would like to migrate your database.
-U is the name of the user present on the source server
-Fc: Sets the output as a custom-format archive suitable for input into pg_restore.
-b: Include large objects in the dump.
-v: Specifies verbose mode
-f: Dump file path
```

## Create a Database on Your Target Instance<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump-create"></a>

First, login to your target database server\.

```
psql -h <hostname> -p 5432 -U <username> -d <database_name>

-h is the name of target server where you would like to migrate your database.
-U is the name of the user present on the target server.
-d is the name of database name present on target already.
```

Then, use the following command to create a database\.

```
create database migrated_database;
```

## Import Dump Files<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump-import"></a>

You can use the following command to import the dump file into your Amazon RDS instance\.

```
pg_restore -v -h <hostname> -U <username> -d <database_name> -j 2 <dumpfilelocation.sql>

-h is the name of target server where you would like to migrate your database.
-U is the name of the user present on the target server.
-d is the name of database name that was created in step 2.
<dumpfilelocation.sql> is the dump file that was created to generate the script of the database using pg_dump
```

## Migrate Database Roles and Users<a name="chap-manageddatabases.postgresql-rds-postgresql-full-load-pd_dump-migrate"></a>

To export such database objects as roles and users, you can use the `pg_dumpall` utility\.

To generate a script for users and roles, run the following command on the source database\.

```
pg_dumpall -U <username> -h <hostname>  -f <dumpfilelocation.sql> --no-role-passwords -g


-h is the name of source server where you would like to migrate your database.
-U is the name of the user present on the source server.
-f: Dump file path.
-g: Dump only global objects (roles and tablespaces), no databases.
```

To restore users and roles, run the following command on your target database\.

```
psql -h <hostname> -U <username> -f <dumpfilelocation.sql>

-h is the name of target server where you would like to migrate your database.
-U is the name of the user present on the target server.
-f: Dump file path.
```

To complete the export and import operations, the pg\_dump and pg\_restore requires some time\. This time depends on the following parameters\.
+ The size of your source database\.
+ The number of jobs\.
+ The resources that you provision for your instance used to invoke pg\_dump and pg\_restore\.