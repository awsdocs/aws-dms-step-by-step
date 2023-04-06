# Troubleshooting<a name="chap-oracle2postgresql.troubleshooting"></a>

The two most common problem areas when working with Oracle as a source and PostgreSQL as a target are: supplemental logging and case sensitivity\.
+ Supplemental logging – With Oracle, in order to replicate change data, supplemental logging must be enabled\. However, if you enable supplemental logging at the database level, it sometimes still needs to be enabled when new tables are created\. The best remedy for this is to allow AWS DMS to enable supplemental logging for you by using the extra connection attribute:

  ```
  addSupplementalLogging=Y
  ```
+ Case sensitivity: Oracle is case\-insensitive \(unless you use quotes around your object names\)\. However, text appears in the upper case\. Thus, AWS DMS defaults to naming your target objects in the upper case\. In most cases, you’ll want to use transformations to change schema, table, and column names to lower case\.

For more tips, see [Troubleshooting migration tasks](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html)\.

To troubleshoot issues specific to Oracle, see [Troubleshooting Oracle Specific Issues](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Oracle)\.

To troubleshoot PostgreSQL issues, see [Troubleshooting PostgreSQL Specific Issues](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.PostgreSQL)\.