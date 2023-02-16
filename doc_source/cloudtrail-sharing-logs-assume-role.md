# Assuming a role<a name="cloudtrail-sharing-logs-assume-role"></a>

You must designate a separate IAM user to assume each role you creat in each account\. You must then ensure that each IAM user has appropriate permissions\.

## IAM users and roles<a name="cloudtrail-sharing-logs-assume-role-iam-user-permission"></a>

After you create the necessary roles and policies in Account A for scenarios 1 and 2, you must designate an IAM user in each of the accounts B, C, and Z\. Each IAM user programmatically assumes the appropriate role to access the log files\. That is, the user in account B assumes the role created for account B, the user in account C assumes the role created for account C, and the user in account Z assumes the role created for account Z\. When a user assumes a role, AWS returns temporary security credentials to that user\. They can then make requests to list, retrieve, copy, or delete log files depending on the permissions granted by the access policy associated with the role\. 

For more information about working with IAM identities, see [IAM Identities \(users, user groups, and roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html)\. 

The primary difference between scenarios 1 and 2 is in the access policy that you create for each IAM role in each scenario\.
+ In scenario 1, the access policies for accounts B and C limit each account to reading only its own log files\. For more information, see [Creating an access policy to grant access to accounts you own](cloudtrail-sharing-logs-your-accounts.md)\.
+ In scenario 2, the access policy for Account Z allows it to read all the log files that are aggregated in the Amazon S3 bucket\. For more information, see [Creating an access policy to grant access to a third party](cloudtrail-sharing-logs-third-party.md)\.

## Creating permissions policies for IAM users<a name="cloudtrail-sharing-logs-assume-role-create-policy"></a>

To perform the actions permitted by a role, the IAM user must have permission to call the AWS STS [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API\. You must edit the policy for each user to grant them the appropriate permissions\. To do this, you set a **Resource** element in the policy that you attach to the IAM user\. The following example shows a policy for an IAM user in Account B that allows the user to assume a role named `Test` created earlier by Account A\. 

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

**To edit a customer managed policy \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\.

1. In the list of policies, choose the policy name of the policy to edit\. You can use the **Filter policies** menu and the search box to filter the list of policies\.

1. Choose the **Permissions** tab, and then choose **Edit policy**\. 

1. Do one of the following:
   + Choose the **Visual editor** tab to change your policy without understanding JSON syntax\. You can make changes to the service, actions, resources, or optional conditions for each permissions block in your policy\. You can also import a policy to add additional permissions to the bottom of your policy\. When you are finished making changes, choose **Review policy** to continue\.
   + Choose the **JSON** tab to modify your policy by entering or pasting text in the JSON text box\. You can also import a policy to add additional permissions to the bottom of your policy\. Resolve any security warnings, errors, or general warnings generated during [policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_policy-validator.html), and then choose **Review policy**\. 
**Note**  
You can switch between the **Visual editor** and **JSON** tabs any time\. However, if you make changes or choose **Review policy** in the **Visual editor** tab, IAM might restructure your policy to optimize it for the visual editor\. For more information, see [Policy restructuring](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_policies.html#troubleshoot_viseditor-restructure) in the *IAM User Guide*\.

1. On the **Review** page, review the policy **Summary** and then choose **Save changes** to save your work\.

1. If the managed policy already has the maximum of five versions, choosing **Save changes** displays a dialog box\. To save your new version, you must remove at least one earlier version\. You cannot delete the default version\. For more information about policy versions, see [Setting the default version of customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-versioning.html#default-version) in the *IAM User Guide*\. Choose from the following options:
   + **Remove oldest nondefault policy version \(version v\# \- created \# days ago\)** – Use this option to see which version will be deleted and when it was created\. You can view the JSON policy document for all nondefault versions by choosing the second option, **Select versions to remove**\. 
   + **Select versions to remove** – Use this option to view the JSON policy document and choose one or more versions to delete\.

   After choosing the versions to remove, choose **Delete version and save** to save your new policy version\.

## Calling AssumeRole<a name="cloudtrail-sharing-logs-assume-role-call"></a>

A user in accounts B, C, or Z can assume a role by creating an application that calls the AWS STS [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API and passes the role session name, the Amazon Resource Number \(ARN\) of the role to assume, and an optional external ID\. The role session name is defined by Account A when it creates the role to assume\. The external ID, if any, is defined by Account Z and passed to Account A for inclusion during role creation\. For more information, see [How to Use an External ID When Granting Access to Your AWS Resources to a Third Party](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) in the *IAM User Guide*\. You can retrieve the ARN from the Account A by opening the IAM console\.

**To find the ARN Value in Account A with the IAM console**

1. Choose **Roles**

1. Choose the role you want to examine\.

1. Look for the **Role ARN** in the **Summary** section\.

The AssumeRole API returns temporary credentials that a user in accounts B, C, or Z can use to access resources in Account A\. In this example, the resources you want to access are the Amazon S3 bucket and the log files that the bucket contains\. The temporary credentials have the permissions that you defined in the role access policy\.

The following Python example \(using the [AWS SDK for Python \(Boto\)](https://aws.amazon.com/tools/)\) shows how to call `AssumeRole` and how to use the temporary security credentials returned to list all Amazon S3 buckets controlled by Account A\.

```
def list_buckets_from_assumed_role(user_key, assume_role_arn, session_name):
    """
    Assumes a role that grants permission to list the Amazon S3 buckets in the account.
    Uses the temporary credentials from the role to list the buckets that are owned
    by the assumed role's account.

    :param user_key: The access key of a user that has permission to assume the role.
    :param assume_role_arn: The Amazon Resource Name (ARN) of the role that
                            grants access to list the other account's buckets.
    :param session_name: The name of the STS session.
    """
    sts_client = boto3.client(
        'sts', aws_access_key_id=user_key.id, aws_secret_access_key=user_key.secret)
    try:
        response = sts_client.assume_role(
            RoleArn=assume_role_arn, RoleSessionName=session_name)
        temp_credentials = response['Credentials']
        print(f"Assumed role {assume_role_arn} and got temporary credentials.")
    except ClientError as error:
        print(f"Couldn't assume role {assume_role_arn}. Here's why: "
              f"{error.response['Error']['Message']}")
        raise

    # Create an S3 resource that can access the account with the temporary credentials.
    s3_resource = boto3.resource(
        's3',
        aws_access_key_id=temp_credentials['AccessKeyId'],
        aws_secret_access_key=temp_credentials['SecretAccessKey'],
        aws_session_token=temp_credentials['SessionToken'])
    print(f"Listing buckets for the assumed role's account:")
    try:
        for bucket in s3_resource.buckets.all():
            print(bucket.name)
    except ClientError as error:
        print(f"Couldn't list buckets for the account. Here's why: "
              f"{error.response['Error']['Message']}")
        raise
```