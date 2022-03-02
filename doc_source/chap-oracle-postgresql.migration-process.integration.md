# Integration with Third\-Party Applications<a name="chap-oracle-postgresql.migration-process.integration"></a>

Few applications are islands and your Oracle application is likely to integrate with other applications that are not themselves going to be migrated\. Examples include ETL, reporting, and monitoring applications for alerts and logs\. For more information, see [Script/ETL/Report Conversion](chap-oracle-postgresql.migration-process.script-conversion.md)\.

If these third\-party applications connect directly to the Oracle database, they are going to be affected by the migration\. If they are packaged applications, the vendor may offer support for Amazon RDS and Aurora PostgreSQL and if they are custom, you may need to modify them to work with the migrated application\. There are a wealth of resources on the partner network which complement any solution from AWS\.

 AWS native tools such as [Amazon Simple Notification Service](https://aws.amazon.com/sns/), [Amazon RDS Performance Insights](https://aws.amazon.com/rds/performance-insights/), [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/), and [Amazon Relational Database Service](http://aws.amazon.com/rds) are already integrated with the Amazon RDS and Aurora PostgreSQL database platform and are recommended for a full picture of the ongoing performance\.

For more information, see [Engage with AWS Partners](https://partners.amazonaws.com/)\.