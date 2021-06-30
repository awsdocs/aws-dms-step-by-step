# Cut over<a name="chap-mariadb2auroramysql.cutover"></a>

After the data validation is complete and any problems resolved, you can load the database triggers, functions, and procedures\.

To do this, use the `routines.sql` file generated from MariaDB to create the necessary routines in Aurora MySQL\. The following statement loads all procedures, functions, and triggers into the Aurora MySQL database\.

```
$ mysql -h mysqltrg-instance-1.xxxxxxxxx.us-east-1.rds.amazonaws.com  -u master -p migration -P 3306 < routines.sql
```

After the routines are loaded, connect to the Aurora MySQL database to validate as shown following\.

```
$ mysql -h mysqltrg-instance-1.xxxxxxxxx.us-east-1.rds.amazonaws.com  -u master -p migration -P 3306
Enter password:
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 957
Server version: 5.6.10 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [migration]> select routine_schema as database_name,
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
3 rows in set (0.002 sec)


MySQL [migration]> select TRIGGER_SCHEMA, TRIGGER_NAME from information_schema.triggers where TRIGGER_SCHEMA='migration';
+----------------+-----------------------+
| TRIGGER_SCHEMA | TRIGGER_NAME          |
+----------------+-----------------------+
| migration      | increment_animal      |
| migration      | contacts_after_update |
+----------------+-----------------------+
2 rows in set (0.009 sec)
```

The preceding output shows that all the procedures, triggers, and functions are loaded successfully to the Aurora MySQL database\.