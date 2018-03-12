# Creating a Role<a name="cloudtrail-sharing-logs-create-role"></a>

When you aggregate log files from multiple accounts into a single Amazon S3 bucket, only the account that has full control of the bucket, Account A in our example, has full read access to all of the log files in the bucket\. Accounts B, C, and Z in our example do not have any rights until granted\. Therefore, to share your AWS CloudTrail log files from one account to another \(that is, to complete either Scenario 1 or Scenario 2 described previously in this section\), you must *enable cross\-account access*\. You can do this by creating IAM roles and their associated access policies\. 

## Roles<a name="cloudtrail-sharing-logs-create-role-roles"></a>

Create an IAM *role* for each account to which you want to give access\. In our example, you will have three roles, one each for accounts B, C, and Z\. Each IAM role defines an access or permissions policy that enables the accounts to access the resources \(log files\) owned by account A\. The permissions are attached to each role and are associated with each account \(B, C, or Z\) only when the role is assumed\. For details about permissions management for IAM roles, see [IAM Roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\. For more information about how to assume a role, see [Assuming a Role](cloudtrail-sharing-logs-assume-role.md)\. 

## Policies<a name="cloudtrail-sharing-logs-create-role-policies"></a>

There are two policies for each IAM role you create\. The *trust policy* specifies a * trusted entity* or *principal*\. In our example, accounts B, C, and Z are trusted entities, and an IAM user with the proper permissions in those accounts can assume the role\. 

The *trust policy* is automatically created when you use the console to create the role\. If you use the SDK to create the role, you must supply the trust policy as a parameter to the `CreateRole` API\. If you use the CLI to create the role, you must specify the trust policy in the `create-role` CLI command\. 

The *role access \(or permissions\) policy* that you must create as the owner of Account A defines what actions and resources the principal or trusted entity is allowed access to \(in this case, the CloudTrail log files\)\. For Scenario 1 that grants log file access to the account that generated the log files, as discussed in [Creating an Access Policy to Grant Access to Accounts You Own](cloudtrail-sharing-logs-your-accounts.md)\. For Scenario 2 that grants read access to all log files to a third party, as discussed in [Creating an Access Policy to Grant Access to a Third Party ](cloudtrail-sharing-logs-third-party.md)\. 

For further details about creating and working with IAM policies, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

## Creating a Role<a name="cloudtrail-sharing-logs-create-role-steps"></a>

**To Create a Role by Using the Console**

1. Sign into the AWS Management Console as an administrator of Account A\.

1. Navigate to the IAM console\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create New Role**\.

1. Type a name for the new role, and then choose **Next Step**\.

1. Choose **Role for Cross\-Account Access**\.

1. For Scenario 1, do the following to provide access between accounts you own:

   1. Choose **Provide access between AWS accounts you own\.**

   1. Enter the twelve\-digit account ID of the account \(B, C, or Z\) to be granted access\.

   1. Check the **Require MFA** box if you want the user to provide multi\-factor authentication before assuming the role\. 

   For Scenario 2, do the following to provide access to a third\-party account\. In our example, you would perform these steps for Account Z, the third\-party log analyzer:

   1. Choose **Allows IAM users from a 3rd party AWS account to access this account\.**

   1. Enter the twelve\-digit account ID of the account \(Account Z\) to be granted access\.

   1. Enter an external ID that provides additional control over who can assume the role\. For more information, see [How to Use an External ID When Granting Access to Your AWS Resources to a Third Party](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) in the *IAM User Guide*\. 

1. Choose **Next Step** to attach a policy that sets the permissions for this role\.

1. Under **Attach Policy**, choose the **AmazonS3ReadOnlyAccess** policy\.
**Note**  
By default, the **AmazonS3ReadOnlyAccess** policy grants retrieval and list rights to all Amazon S3 buckets within your account\.

   + To grant an account access to only that account's log files \(Scenario 1\), see [Creating an Access Policy to Grant Access to Accounts You Own](cloudtrail-sharing-logs-your-accounts.md)\.

   +  To grant an account access to all of the log files in the Amazon S3 bucket \(Scenario 2\), see [Creating an Access Policy to Grant Access to a Third Party ](cloudtrail-sharing-logs-third-party.md)\.

1. Choose **Next Step**

1. Review the role information\. 
**Note**  
You can edit the role name at this point if you wish, but if you do so, you will be taken back to the **Step 2: Select Role Type** page where you must reenter the information for the role\. 

1. Choose **Create Role**\. When the role creation process completes, the role you created appears in the role list\.