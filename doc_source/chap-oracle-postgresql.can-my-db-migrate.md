# Can My Oracle Database Migrate?<a name="chap-oracle-postgresql.can-my-db-migrate"></a>

To quickly see if your workload qualifies as a migration candidate, please use the **DMA Connect Application and Database Questionnaire** to sort out migration obstacles specific to your application\. Consider the following questions\. The more you answer **No**, the easier the migration to Amazon RDS for PostgreSQL or Aurora PostgreSQL will be\.


| Application Questions | Comments | 
| --- | --- | 
|  Are there Oracle dependent parts of the application that you can’t modify by yourself?  |  If you don’t control all of the code it can be difficult to change the underlying database\.  | 
|  Is the application commercial off the shelf, and not available for PostgreSQL?  |  Unless the COTS application also supports Oracle, it will not be able to migrate\.  | 
|  Does the application use specific methods to connect to an Oracle database such as Oracle Call Interface \(OCI\)?  |  Refactoring OCI calls to ODBC is not impossible, but typically an involved process\.  | 
|  Does the application use Oracle specific libraries?  |  It could be challenging finding PostreSQL replacements for Oracle specific libraries\.  | 


| Database Questions | Comments | 
| --- | --- | 
|  Does the database use any third party packages?  |  It could be challenging finding PostreSQL replacements for Oracle specific packages\.  | 
|  Does the database use any data cartridges?  |  It could be challenging finding PostreSQL replacements for Oracle specific cartridges\.  | 
|  Does the application use Oracle Forms or Application Express \(APEX\)?  |  Completely replacing Forms or APEX with a non\-Oracle solution is substantial\.  | 
|  Does the database use SQLJ or \.NET stored procedures?  |  You can refactor external stored procedure code for use with PostgreSQL, but it adds development work\.  | 
|  Does the database use Oracle Streams?  |  Some refactoring is required to replace Oracle Streams with a PostgreSQL\-compatible solution\.  | 
|  Does the database use Oracle Multi Media?  |  Some refactoring is required to replace Oracle Multi Media with a PostgreSQL\-compatible solution\.  | 
|  Does the database use Oracle Locator?  |  Depending on feature use, such a solution may be refactored to work with PostGIS 3\.1\.  | 
|  Does the database use Oracle Java Virtual Machine \(JVM\)?  |  Detaching a Java application from Oracle JVM can be involved development work\.  | 
|  Does the database use Oracle Machine Learning or formerly Advanced Analytics?  |  The solution will have to be refactored to use similar functionality on AWS\.  | 