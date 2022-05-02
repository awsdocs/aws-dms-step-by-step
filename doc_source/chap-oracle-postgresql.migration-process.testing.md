# Testing and Bug Fixing<a name="chap-oracle-postgresql.migration-process.testing"></a>

Testing can be manual or automated\. We recommend that you use an automated framework for testing\. During migration, you will need to run the test multiple times, so having an automated testing framework helps speed up the bug fixing and optimization cycles\.

## Unit Testing<a name="chap-oracle-postgresql.migration-process.testing.unit"></a>

Unit testing at the data level after migration can range from comparing every last bit of data in source and target by comparing extracted CSV files, but more realistically, custom aggregation queries should be constructed to incorporate large amounts of the migrated data and compare the results\.

Unit tests validate individual units of your code, independent from any other components\. Unit tests check the smallest unit of functionality and should have few reasons to fail\.

Database objects need to be validated after migrating the DDL of an Oracle database to PostgreSQL\. Database objects includes packages, tables, views, sequences, triggers, primary and foreign keys, indexes, constraints\.

A typical way to perform unit testing on the converted database is to script out calls to stored procedures and functions and compare the returned data with external tools such as standard Linux/Unix tooling of diff\.

The application needs to be validated with new and existing test case scenarios based on documented changes on database objects such as field names, types, minimum and maximum values, length, mandatory fields, field level validations etc\.

## Functional Testing<a name="chap-oracle-postgresql.migration-process.testing.functional"></a>

Functional testing of the application is done by exercising user stories and comparing the results on the source and target system\. This is typically a manual process, but third\-party tools do exist to make automated regression tests of the UI \(e\.g\. Selenium\)\.

Functional testing of the database is largely done through the application, but there may be additional direct database use cases that can only be done directly on the database such as ETL for imports and extracts\. In these cases, the data can be compared automatically before and after using standard Linux/Unix tooling like diff on extracted CSV files for example\.

Functional testing of reports involve visual inspection to see that all fields are correctly displayed and comparison of the semantic values between the old and the new reports\.

## Load Testing<a name="chap-oracle-postgresql.migration-process.testing.load"></a>

In order to stress the migrated system and test its performance you may perform load testing which is typically done on a system that is scaled the same as production and requires a means of simulating load on the system\. It is sometimes limited to running specific well\-known expensive operations rather than user traffic\.

## Standard Operating Procedures<a name="chap-oracle-postgresql.migration-process.testing.standard"></a>

Standard Operating Procedures \(SOP\) may be affected by a migration of database application\. Database management procedures change when going to PostgreSQL and some procedures may be unnecessary when going to the highly managed Aurora PostgreSQL\.

In any case, all existing operational procedures need to be tested and their language updated to reflect the new environment\. For more information, see [Managing Amazon Aurora PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.Managing.html)\.

## Monitoring<a name="chap-oracle-postgresql.migration-process.testing.monitoring"></a>

Monitoring of the database will be affected by the migration and some metrics may change which could affect how SLA is monitored\. The way operational staff go from detecting a problem to diving into the underlying details may be affected\. For more information, see [Monitor Amazon RDS for PostgreSQL and Amazon Aurora for PostgreSQL database log errors and set up notifications using Amazon CloudWatch](https://aws.amazon.com/blogs/database/monitor-amazon-rds-for-postgresql-and-amazon-aurora-for-postgresql-database-log-errors-and-set-up-notifications-using-amazon-cloudwatch/)\.

## Cutover<a name="chap-oracle-postgresql.migration-process.testing.cutover"></a>

Cutover procedures are the planned event where everything goes the way you want, but it still needs to be tested\.

## Fallback<a name="chap-oracle-postgresql.migration-process.testing.fallback"></a>

Fallback is when you have both old and new systems in sync with the new one operating as primary and you decide to switch back to the original which is still in sync\.

## Rollback<a name="chap-oracle-postgresql.migration-process.testing.rollback"></a>

Rollback is usually the scenario when you don’t have an ongoing replication mechanism to keep old and new in sync, so in the event of a no\-go decision during the cutover, you abandon the new system and go back to the original\.

## Migrate Back<a name="chap-oracle-postgresql.migration-process.testing.migrate-back"></a>

In some rare cases, you may decide to include the option of migrating production from the new system back to the old system after having the cutover\. If you include this scenario, it must be tested\.

For more information, see [Testing Amazon Aurora PostgreSQL by using fault injection queries](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.Managing.FaultInjectionQueries.html), [Automate benchmark tests for Amazon Aurora PostgreSQL](https://aws.amazon.com/blogs/database/automate-benchmark-tests-for-amazon-aurora-postgresql/), [Validating database objects after migration](https://aws.amazon.com/blogs/database/validating-database-objects-after-migration-using-aws-sct-and-aws-dms/), and [Validate database objects after migrating from Oracle to Amazon Aurora PostgreSQL](https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/validate-database-objects-after-migrating-from-oracle-to-amazon-aurora-postgresql.html)\.