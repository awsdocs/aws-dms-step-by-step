# Summary<a name="chap-manageddatabases.postgresql-rds-postgresql-summary"></a>

This document describes the hybrid approach for migrating between PostgreSQL databases\. We analyzed three options for full load and demonstrated the relative performance of each for a test database\.

If you need to create secondary database objects, then pg\_dump and pg\_restore is the most appropriate option\. However, this option incurs a performance tradeoff compared to other options\.

pglogical has a slight performance advantage over publisher and subscriber\. However, you need to install the pglogical extension on your source database server\.

You can use these guidelines to choose the option that best matches your migration goal\.