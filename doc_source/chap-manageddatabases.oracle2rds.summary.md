# Summary<a name="chap-manageddatabases.oracle2rds.summary"></a>

To migrate database objects and data, use either Oracle Export/Import or Oracle Data Pump\. Oracle Export/Import and Oracle Data Pump automate schema object creation\. Oracle Data Pump has better performance than Oracle Export/Import and it’s a newer version of Oracle Export/Import\.

You can still choose to work with Oracle Export/Import for relatively small data sets which are less than 10 GB because of the ease of use\. To migrate table data only, choose any native full load option described in the full load and performance comparison sections\.

For full load and ongoing replication, use the hybrid approach\. AWS DMS recommends using Oracle Data Pump for full load because it’s faster than other tools, and it automates target object creation\.