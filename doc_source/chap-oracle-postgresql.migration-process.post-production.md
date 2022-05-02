# Post\-Production Support<a name="chap-oracle-postgresql.migration-process.post-production"></a>

After production cutover, there are a few possibilities for what can happen\. You will have either decided to fix forward as the old system is being decommissioned, or you have decided to way a certain amount of time, a bake\-in time, with production on the new system, during which a decision to abandon the new system can be made\. Abandoning the new system has the following flavors:
+ Roll back, all new data is lost\.
+ Roll back and reapply all new transactions\.
+ Roll back and migrate new production data back\.
+ Maintain a live replication back to the old system until bake\-in period is over\.

During this time, defects are tracked and triaged for possibly triggering the rollback or being fixed forward\. Help desk will have been trained in the new system differences and will be able to detect if an end user inquiry just requires training or it may be a defect\.

Beyond acceptance migration criteria, the application may have well defined KPIs defined already which can be observed when in production on the new system and compared to historical KPIs\.

For more information, see [How to Migrate Your Oracle Database to PostgreSQL](https://aws.amazon.com/blogs/database/how-to-migrate-your-oracle-database-to-postgresql/)\.