# Granting Permissions for CloudTrail Administration<a name="grant-permissions-for-cloudtrail-administration"></a>

To allow users to administer a CloudTrail trail, you must grant explicit permissions to IAM users to perform the actions associated with CloudTrail tasks\. For most scenarios, you can do this using an AWS managed policy that contains predefined permissions\.

**Note**  
The permissions you grant to users to perform CloudTrail administration tasks are not the same as the permissions that CloudTrail itself requires in order to deliver log files to Amazon S3 buckets or send notifications to Amazon SNS topics\. For more information about those permissions, see [Getting and Viewing Your CloudTrail Log Files](get-and-view-cloudtrail-log-files.md)\. CloudTrail also requires a role that it can assume to deliver events to an Amazon CloudWatch Logs log group\. For more information, see [Granting Custom Permissions for CloudTrail Users](grant-custom-permissions-for-cloudtrail-users.md)\.

A typical approach is to create an IAM group that has the appropriate permissions and then add individual IAM users to that group\. For example, you might create an IAM group for users who should have full access to CloudTrail actions, and a separate group for users who should be able to view trail information but not create or change trails\.

**To create an IAM group and users for CloudTrail access**

1.  Open the IAM console at [https://console\.aws\.amazon\.com/iam](https://console.aws.amazon.com/iam)\.

1. From the dashboard, choose **Groups** in the navigation pane, and then choose **Create New Group**\. 

1. Type a name, and then choose **Next Step**\. 

1. On the **Attach Policy** page, find and choose one of the following policies for CloudTrail:
   +  **AWSCloudTrailFullAccess**\. This policy gives users in the group full access to CloudTrail actions\. These users have permissions to manage the Amazon S3 bucket, the log group for CloudWatch Logs, and an Amazon SNS topic for a trail\.
**Note**  
The **AWSCloudTrailFullAccess** policy is not intended to be shared broadly across your AWS account\. Instead, it should be restricted to AWS account administrators due to the highly sensitive information collected by CloudTrail\. Users with this role have the ability to disable or reconfigure the most sensitive and important auditing functions in their AWS accounts\. For this reason, access to this policy should be closely controlled and monitored\.
   +  **AWSCloudTrailReadOnlyAccess**\. This policy lets users in the group view the CloudTrail console, including recent events and event history\. These users can also view existing trails and their buckets\. Users can download a file of event history, but they cannot create or update trails\.
**Note**  
You can also create a custom policy that grants permissions to individual actions\. For more information, see [Granting Custom Permissions for CloudTrail Users](grant-custom-permissions-for-cloudtrail-users.md)\.

1. Choose **Next Step**\.

1. Review the information for the group you are about to create\.
**Note**  
You can edit the group name, but you will need to choose the policy again\.

1. Choose **Create Group**\. The group that you created appears in the list of groups\.

1. Choose the group name that you created, choose **Group Actions**, and then choose **Add Users to Group**\. 

1. On the **Add Users to Group** page, choose the existing IAM users, and then choose **Add Users**\. If you don't already have IAM users, choose **Create New Users**, enter user names, and then choose **Create**\. 

1. If you created new users, choose **Users** in the navigation pane and complete the following for each user: 

   1. Choose the user\.

   1. If the user will use the console to manage CloudTrail, in the **Security Credentials** tab, choose **Manage Password**, and then create a password for the user\. 

   1. If the user will use the CLI or API to manage CloudTrail, and if you didn't already create access keys, in the **Security Credentials** tab, choose **Manage Access Keys** and then create access keys\. Store the keys in a secure location\.

   1. Give each user his or her credentials \(access keys or password\)\.

## Additional Resources<a name="cloudtrail-notifications-more-info-3"></a>

To learn more about creating IAM users, groups, policies, and permissions, see [Creating an Admins Group Using the Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/GSGHowToCreateAdminsGroup.html) and [Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. 