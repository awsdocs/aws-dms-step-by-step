# Next Steps<a name="schema-conversion-sql-server-aurora-postgresql-next-steps"></a>

After you migrate your SQL Server database to Aurora PostgreSQL using DMS Schema Conversion, you can explore several other resources:
+ Use AWS DMS to migrate your source data\. For more information, see the [Database Migration Service User Guide](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html)\.
+ Use DMS Fleet Advisor to inventory your source databases and discover other candidates to move to the cloud\. For more information, see the [DMS Fleet Advisor User Guide](https://docs.aws.amazon.com/dms/latest/userguide/CHAP_FleetAdvisor.html)\.
+ Learn more about Aurora PostgreSQL\. For more information, see the [Amazon Aurora User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)\.

After you’ve finished using DMS Schema Conversion, clean up your resources\. Amazon terminates the schema conversion instance that your migration project uses in three days after you complete the conversion\. You can retrieve your converted schema and assessment report from the Amazon S3 bucket that you use for DMS Schema Conversion\. However, you need to terminate other resources manually\.

 **To clean up your DMS Schema Conversion resources** 
+ Sign in to the AWS Management Console and open the AWS DMS console\.
+ In the navigation pane, choose **Migration projects**, and then choose your migration project\. Choose **Schema conversion**, and then choose **Stop schema conversion**\. Choose **Delete** and confirm your choice\.
+ Choose **Instance profiles**, and then choose `sc-instance`\. Choose **Delete** and confirm your choice\.
+ Choose **Data providers**, and then select `sc-sql-server` and `sc-postgresql`\. Choose **Delete** and confirm your choice\.

Also, make sure that you delete your Amazon S3 bucket, database secrets in AWS Secrets Manager, IAM roles, and virtual private cloud \(VPC\)\.