# Step 11: Delete Walkthrough Resources<a name="chap-rdsoracle2redshift.steps.deleteresources"></a>

After you have completed this walkthrough, perform the following steps to avoid being charged further for AWS resources used in the walkthrough\. It’s necessary that you do the steps in order, because some resources cannot be deleted if they have a dependency upon another resource\.

To delete AWS DMS resources, do the following:

1. On the navigation pane, choose **Tasks**, choose your migration task \(`migratehrschema`\), and then choose **Delete**\.

1. On the navigation pane, choose **Endpoints**, choose the Oracle source endpoint \(`orasource`\), and then choose **Delete**\.

1. Choose the Amazon Redshift target endpoint \(`redshifttarget`\), and then choose **Delete**\.

1. On the navigation pane, choose **Replication instances**, choose the replication instance \(`DMSdemo-repserver`\), and then choose **Delete**\.

Next, you must delete your AWS CloudFormation stack, `DMSdemo`\. Do the following:

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

   If you are signed in as an IAM user, you must have the appropriate permissions to access AWS CloudFormation\.

1. Choose your CloudFormation stack, `OracletoRedshiftDWusingDMS`\.

1. For **Actions**, choose **Delete stack**\.

The status of the stack changes to DELETE\_IN\_PROGRESS while AWS CloudFormation cleans up the resources associated with the `OracletoRedshiftDWusingDMS` stack\. When AWS CloudFormation is finished cleaning up resources, it removes the stack from the list\.