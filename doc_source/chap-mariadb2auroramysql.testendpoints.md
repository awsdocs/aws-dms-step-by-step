# Test the endpoints<a name="chap-mariadb2auroramysql.testendpoints"></a>

1. On the navigation pane, choose **Endpoints**\.

1. Choose the source endpoint name \(`maria-on-prem`\) and do the following:

   1. Choose **Test connections**\.

   1. Choose the replication instance to test \(`mariadb-mysql`\)\.

   1. Choose **Run Test** and wait for the status to be **successful**\.

1. On the navigation pane, choose **Endpoints**\.

1. Choose the target endpoint name \(`mysqltrg-rds`\) and do the following:

   1. Choose **Test Connections**\.

   1. Choose the replication instance to test \(`mariadb-mysql`\)\.

   1. Choose **Run Test** and wait for the status to be **successful**\.

**Note**  
If **Run Test** returns a status other than **successful**, the reason for the failure is displayed\. Make sure that you resolve the issue before proceeding further\.