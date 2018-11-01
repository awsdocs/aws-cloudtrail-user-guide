# Assuming a Role<a name="cloudtrail-sharing-logs-assume-role"></a>

You must designate a separate IAM user to assume each role you've created in each account, and ensure that each IAM user has appropriate permissions\.

## IAM Users and Roles<a name="cloudtrail-sharing-logs-assume-role-iam-user-permission"></a>

After you have created the necessary roles and policies in Account A for scenarios 1 and 2, you must designate an IAM user in each of the accounts B, C, and Z\. Each IAM user will programmatically assume the appropriate role to access the log files\. That is, the user in account B will assume the role created for account B, the user in account C will assume the role created for account C, and the user in account Z will assume the role created for account Z\. When a user assumes a role, AWS returns temporary security credentials that can be used to make requests to list, retrieve, copy, or delete the log files depending on the permissions granted by the access policy associated with the role\. 

For more information about working with IAM users, see [ Working with IAM Users and Groups ](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html)\. 

The primary difference between scenarios 1 and 2 is in the access policy that you create for each IAM role in each scenario\.
+ In scenario 1, the access policies for accounts B and C limit each account to reading only its own log files\. For more information, see [Creating an Access Policy to Grant Access to Accounts You Own](cloudtrail-sharing-logs-your-accounts.md)\.
+ In scenario 2, the access policy for Account Z allows it to read all the log files that are aggregated in the Amazon S3 bucket\. For more information, see [Creating an Access Policy to Grant Access to a Third Party ](cloudtrail-sharing-logs-third-party.md)\.

## Creating permissions policies for IAM users<a name="cloudtrail-sharing-logs-assume-role-create-policy"></a>

To perform the actions permitted by the roles, the IAM user must have permission to call the AWS STS [ `AssumeRole` ](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API\. You must edit the *user\-based policy* for each IAM user to grant them the appropriate permissions\. That is, you set a **Resource** element in the policy that is attached to the IAM user\. The following example shows a policy for an IAM user in Account B that allows the user to assume a role named "Test" created earlier by Account A\. 

**To attach the required policy to the IAM role**

1. Sign in to the AWS Management Console and open the IAM console\.

1. Choose the user whose permissions you want to modify\. 

1. Choose the **Permissions** tab\.

1. Choose **Custom Policy**\.

1. Choose **Use the policy editor to customize your own set of permissions**\.

1. Type a name for the policy\.

1. Copy the following policy into the space provided for the policy document\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["sts:AssumeRole"],
      "Resource": "arn:aws:iam::account-A-id:role/Test"
    }
  ]
}
```

**Important**  
Only IAM users can assume a role\. If you attempt to use AWS root account credentials to assume a role, access will be denied\. 

## Calling AssumeRole<a name="cloudtrail-sharing-logs-assume-role-call"></a>

A user in accounts B, C, or Z can assume a role by creating an application that calls the AWS STS [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API and passes the role session name, the Amazon Resource Number \(ARN\) of the role to assume, and an optional external ID\. The role session name is defined by Account A when it creates the role to assume\. The external ID, if any, is defined by Account Z and passed to Account A for inclusion during role creation\. For more information, see [How to Use an External ID When Granting Access to Your AWS Resources to a Third Party](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) in the *IAM User Guide*\. You can retrieve the ARN from the Account A by opening the IAM console\.

**To find the ARN Value in Account A with the IAM console**

1. Choose **Roles**

1. Choose the role you want to examine\.

1. Look for the **Role ARN** in the **Summary** section\.

The AssumeRole API returns temporary credentials that a user in accounts B, C, or Z can use to access resources in Account A\. In this example, the resources you want to access are the Amazon S3 bucket and the log files that the bucket contains\. The temporary credentials have the permissions that you defined in the role access policy\.

The following Python example \(using the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/tools/)\) shows how to call `AssumeRole` and how to use the temporary security credentials returned to list all Amazon S3 buckets controlled by Account A\.

```
import boto
from boto.sts import STSConnection
from boto.s3.connection import S3Connection

# The calls to AWS STS AssumeRole must be signed using the access key ID and secret
# access key of an IAM user or using existing temporary credentials. (You cannot call
# AssumeRole using the access key for an account.) The credentials can be in 
# environment variables or in a configuration file and will be discovered automatically
# by the STSConnection() function. For more information, see the Python SDK 
# documentation: http://boto.readthedocs.org/en/latest/boto_config_tut.html

sts_connection = STSConnection()
assumedRoleObject = sts_connection.assume_role(
    role_arn="arn:aws:iam::account-of-role-to-assume:role/name-of-role",
    role_session_name="AssumeRoleSession1"
)

# Use the temporary credentials returned by AssumeRole to call Amazon S3  
# and list the bucket in the account that owns the role (the trusting account)
s3_connection = S3Connection(
    aws_access_key_id=assumedRoleObject.credentials.access_key,
    aws_secret_access_key=assumedRoleObject.credentials.secret_key,
    security_token=assumedRoleObject.credentials.session_token
)
bucket = s3_connection.get_bucket(bucketname)
print bucket.name
```