# Validate the migration<a name="chap-mariadb2auroramysql.validate"></a>

 AWS DMS performs data validation to confirm that your data successfully migrated the source database to the target\. You can check the **Table statistics** page to determine the DML changes that occurred after the AWS DMS task started\. During data validation, AWS DMS compares each row in the source with its corresponding row at the target, and verifies that those rows contain the same data\. To accomplish this, AWS DMS issues the appropriate queries to retrieve the data\.

After your data is loaded successfully, you can select your task on the AWS DMS page and choose **Table statistics** to show statistics about your migration\. The following screen shot shows the **Table statistics** page and its relevant entries\.

The following screenshot shows the table statics page and its relevant entries\.

![\[Table statistics\]](http://docs.aws.amazon.com/dms/latest/sbs/images/sbs-mariadb2aurmysql-validation.png)

 AWS DMS can validate the data between source and target engines\. The **Validation state **column helps us to validate the data migration\. This ensures that your data was migrated accurately from the source to the target\.