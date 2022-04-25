# Stop sharing CloudTrail log files between AWS accounts<a name="cloudtrail-sharing-logs-stop-sharing"></a>

To stop sharing log files to another AWS account, simply delete the role that you created for that account in [Creating a role](cloudtrail-sharing-logs-create-role.md)\. 

1. Sign in to the AWS Management Console as an IAM user with administrative\-level permissions for Account A\. 

1. Navigate to the IAM console\.

1. In the navigation pane, click **Roles**\.

1. Select the role you want to delete\.

1. Right\-click and select **Delete Role** from the context menu\.