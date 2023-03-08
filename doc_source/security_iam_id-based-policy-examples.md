# Identity\-based policy examples for AWS CloudTrail<a name="security_iam_id-based-policy-examples"></a>

By default, users and roles don't have permission to create or modify CloudTrail resources\. They also can't perform tasks by using the AWS Management Console, AWS Command Line Interface \(AWS CLI\), or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform actions on the resources that they need\. The administrator must then attach those policies for users that require them\.

To learn how to create an IAM identity\-based policy by using these example JSON policy documents, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) in the *IAM User Guide*\.

For details about actions and resource types defined by CloudTrail, including the format of the ARNs for each of the resource types, see [Actions, Resources, and Condition Keys for AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html) in the *Service Authorization Reference*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Example: Allowing and denying actions for a specified trail](#security_iam_id-based-policy-examples-allow-deny-for-specific-trail)
+ [Examples: Creating and applying policies for actions on specific trails](#grant-custom-permissions-for-cloudtrail-users-resource-level)
+ [Examples: Denying access to create or delete event data stores based on tags](#security_iam_id-based-policy-examples-eds-tags)
+ [Using the CloudTrail console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Granting custom permissions for CloudTrail users](#grant-custom-permissions-for-cloudtrail-users)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete CloudTrail resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

CloudTrail doesn't have service\-specific context keys that you can use in the `Condition` element of policy statements\.

## Example: Allowing and denying actions for a specified trail<a name="security_iam_id-based-policy-examples-allow-deny-for-specific-trail"></a>

The following example demonstrates a policy that allows users with the policy to view the status and configuration of a trail and start and stop logging for a trail named *My\-First\-Trail*\. This trail was created in the US East \(Ohio\) Region \(its home region\) in the AWS account with the ID *123456789012*\.

```
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "cloudtrail:StartLogging",
              "cloudtrail:StopLogging",
              "cloudtrail:GetTrail",
              "cloudtrail:GetTrailStatus",
              "cloudtrail:GetEventSelectors"
          ],
          "Resource": [
              "arn:aws:cloudtrail:us-east-2:123456789012:trail/My-First-Trail"
          ]
      }
  ]
}
```

The following example demonstrates a policy that explicitly denies CloudTrail actions for any trail not named *My\-First\-Trail*\.

```
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Deny",
          "Action": [
              "cloudtrail:*"
          ],
          "NotResource": [
              "arn:aws:cloudtrail:us-east-2:123456789012:trail/My-First-Trail"
          ]
      }
  ]
}
```

## Examples: Creating and applying policies for actions on specific trails<a name="grant-custom-permissions-for-cloudtrail-users-resource-level"></a>

You can use permissions and policies to control a user's ability to perform specific actions on CloudTrail trails\. 

For example, you don't want users of your company’s developer group to start or stop logging on a specific trail\. However, you might want to grant them permission to perform the `DescribeTrails` and `GetTrailStatus` actions on the trail\. You want the users of the developer group to perform the `StartLogging` or `StopLogging` actions on trails that they manage\. 

You can create two policy statements and attach them to the developer group you create in IAM\. For more information about groups in IAM, see [IAM Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) in the *IAM User Guide*\.

In the first policy, you deny the `StartLogging` and `StopLogging` actions for the trail ARN that you specify\. In the following example, the trail ARN is `arn:aws:cloudtrail:us-east-2:123456789012:trail/Example-Trail`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1446057698000",
            "Effect": "Deny",
            "Action": [
                "cloudtrail:StartLogging",
                "cloudtrail:StopLogging"
            ],
            "Resource": [
                "arn:aws:cloudtrail:us-east-2:123456789012:trail/Example-Trail"
            ]
        }
    ]
}
```

In the second policy, the `DescribeTrails` and `GetTrailStatus` actions are allowed on all CloudTrail resources:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1446072643000",
            "Effect": "Allow",
            "Action": [
                "cloudtrail:DescribeTrails",
                "cloudtrail:GetTrail",
                "cloudtrail:GetTrailStatus"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

If a user of the developer group tries to start or stop logging on the trail that you specified in the first policy, that user gets an access denied exception\. Users of the developer group can start and stop logging on trails that they create and manage\.

The following examples show that the configured developer group in an AWS CLI profile named `devgroup`\. First, a user of `devgroup` runs the `describe-trails` command\. 

```
$ aws --profile devgroup cloudtrail describe-trails
```

The command complete successfully with the following output:

```
{
    "trailList": [
        {
            "IncludeGlobalServiceEvents": true, 
            "Name": "Default", 
            "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/Example-Trail", 
            "IsMultiRegionTrail": false, 
            "S3BucketName": "myS3bucket ", 
            "HomeRegion": "us-east-2"
        }
    ]
}
```

The user then runs the `get-trail-status` command on the trail that you specified in the first policy\.

```
$ aws --profile devgroup cloudtrail get-trail-status --name Example-Trail
```

The command complete successfully with the following output:

```
{
    "LatestDeliveryTime": 1449517556.256, 
    "LatestDeliveryAttemptTime": "2015-12-07T19:45:56Z", 
    "LatestNotificationAttemptSucceeded": "", 
    "LatestDeliveryAttemptSucceeded": "2015-12-07T19:45:56Z", 
    "IsLogging": true, 
    "TimeLoggingStarted": "2015-12-07T19:36:27Z", 
    "StartLoggingTime": 1449516987.685, 
    "StopLoggingTime": 1449516977.332, 
    "LatestNotificationAttemptTime": "", 
    "TimeLoggingStopped": "2015-12-07T19:36:17Z"
}
```

Next, a user in the `devgroup` group runs the `stop-logging` command on the same trail\. 

```
$ aws --profile devgroup cloudtrail stop-logging --name Example-Trail
```

The command returns an access denied exception, such as the following:

```
A client error (AccessDeniedException) occurred when calling the StopLogging operation: Unknown
```

The user runs the `start-logging` command on the same trail\. 

```
$ aws --profile devgroup cloudtrail start-logging --name Example-Trail
```

Again the command returns an access denied exception, such as the following:

```
A client error (AccessDeniedException) occurred when calling the StartLogging operation: Unknown 
```

## Examples: Denying access to create or delete event data stores based on tags<a name="security_iam_id-based-policy-examples-eds-tags"></a>

In the following policy example, permission to create an event data store with `CreateEventDataStore` is denied if at least one of the following conditions aren't met:
+ The event data store doesn't have a tag key of `stage` applied to itself
+ The value of the stage tag isn't `alpha`, `beta`, `gamma`, or `prod`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "cloudtrail:CreateEventDataStore",
            "Resource": "*",
            "Condition": {
                "Null": {
                    "aws:RequestTag/stage": "true"
                }
            }
        },
        {
            "Effect": "Deny",
            "Action": "cloudtrail:CreateEventDataStore",
            "Resource": "*",
            "Condition": {
                "ForAnyValue:StringNotEquals": {
                    "aws:RequestTag/stage": [
                        "alpha",
                        "beta",
                        "gamma",
                        "prod"
                    ]
                }
            }
        }
    ]
}
```

In the following policy example, permission to delete an event data store with `DeleteEventDataStore` is denied is if the event data store has a `stage` tag with a value of `prod`\. A policy like this one can help protect an event data store from accidental deletion\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "cloudtrail:DeleteEventDataStore",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/stage": "prod"
                }
            }
        }
    ]
}
```

## Using the CloudTrail console<a name="security_iam_id-based-policy-examples-console"></a>

To access the AWS CloudTrail console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the CloudTrail resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(users or roles\) with that policy\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.

### Granting permissions for CloudTrail administration<a name="grant-permissions-for-cloudtrail-administration"></a>

To allow IAM roles or users to administer a CloudTrail resource, such as a trail, event data store, or channel, you must grant explicit permissions to perform the actions associated with CloudTrail tasks\. In most situations, you can use an AWS managed policy that contains predefined permissions\.

**Note**  
The permissions you grant to users to perform CloudTrail administration tasks aren't the same as the permissions that CloudTrail requires to deliver log files to Amazon S3 buckets or send notifications to Amazon SNS topics\. For more information about those permissions, see [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.  
If you configure integration with Amazon CloudWatch Logs, CloudTrail also requires a role that it can assume to deliver events to an Amazon CloudWatch Logs log group\. You must create the role that CloudTrail uses\. For more information, see [Granting permission to view and configure Amazon CloudWatch Logs information on the CloudTrail console](#grant-cloudwatch-permissions-for-cloudtrail-users) and [Sending events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\.

The following AWS managed policies are available for CloudTrail:
+  [https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudTrail_FullAccess.html](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudTrail_FullAccess.html) – This policy provides full access to CloudTrail actions on CloudTrail resources, such as trails, event data stores, and channels\. This policy provides the required permissions to create, update, and delete CloudTrail trails, event data stores, and channels\. 

   This policy also provides permissions to manage the Amazon S3 bucket, the log group for CloudWatch Logs, and an Amazon SNS topic for a trail\. However, the `AWSCloudTrail_FullAccess` managed policy doesn't provide permissions to delete the Amazon S3 bucket, the log group for CloudWatch Logs, or an Amazon SNS topic\. For information about managed policies for other AWS services, see the [https://docs.aws.amazon.com/aws-managed-policy/latest/reference/about-managed-policy-reference.html](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/about-managed-policy-reference.html)\.
**Note**  
The **AWSCloudTrail\_FullAccess** policy isn't intended to be shared broadly across your AWS account\. Users with this role can turn off or reconfigure the most sensitive and important auditing functions in their AWS accounts\. For this reason, you must only apply this policy to account administrators\. You must closely control and monitor use of this policy\.
+  [https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudTrail_ReadOnlyAccess.html](https://docs.aws.amazon.com/aws-managed-policy/latest/reference/AWSCloudTrail_ReadOnlyAccess.html) – This policy grants permissions to view the CloudTrail console, including recent events and event history\. This policy also allows you to view existing trails, event data stores, and channels\. Roles and users with this policy can [download the event history](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events-console.html#downloading-events), but they can't create or update trails, event data stores, or channels\.

To provide access, add permissions to your users, groups, or roles:
+ Users and groups in AWS IAM Identity Center \(successor to AWS Single Sign\-On\):

  Create a permission set\. Follow the instructions in [Create a permission set](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocreatepermissionset.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.
+ Users managed in IAM through an identity provider:

  Create a role for identity federation\. Follow the instructions in [Creating a role for a third\-party identity provider \(federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\.
+ IAM users:
  + Create a role that your user can assume\. Follow the instructions in [Creating a role for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) in the *IAM User Guide*\.
  + \(Not recommended\) Attach a policy directly to a user or add a user to a user group\. Follow the instructions in [Adding permissions to a user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

#### Additional resources<a name="cloudtrail-notifications-more-info-3"></a>

To learn more about using IAM to give identities, such as users and roles, access to resources in your account, see [Getting set up with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-set-up.html) and [Access management for AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\. 

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.

## Allow users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Granting custom permissions for CloudTrail users<a name="grant-custom-permissions-for-cloudtrail-users"></a>

CloudTrail policies grant permissions to users who work with CloudTrail\. If you need to grant different permissions to users, you can attach a CloudTrail policy to an IAM group or to a user\. You can edit the policy to include or exclude specific permissions\. You can also create your own custom policy\. Policies are JSON documents that define the actions a user is allowed to perform and the resources that the user is allowed to perform those actions on\. For specific examples, see [Example: Allowing and denying actions for a specified trail](#security_iam_id-based-policy-examples-allow-deny-for-specific-trail) and [Examples: Creating and applying policies for actions on specific trails](#grant-custom-permissions-for-cloudtrail-users-resource-level)\.

**Contents**
+ [Read\-only access](#grant-custom-permissions-for-cloudtrail-users-read-only)
+ [Full access](#grant-custom-permissions-for-cloudtrail-users-full-access)
+ [Granting permission to view AWS Config information on the CloudTrail console](#grant-aws-config-permissions-for-cloudtrail-users)
+ [Granting permission to view and configure Amazon CloudWatch Logs information on the CloudTrail console](#grant-cloudwatch-permissions-for-cloudtrail-users)
+ [Additional information](#cloudtrail-notifications-more-info-2)

### Read\-only access<a name="grant-custom-permissions-for-cloudtrail-users-read-only"></a>

The following example shows a policy that grants read\-only access to CloudTrail trails\. This is equivalent to the managed policy **AWSCloudTrail\_ReadOnlyAccess**\. It grants users permission to see trail information, but not to create or update trails\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cloudtrail:Get*",
                "cloudtrail:Describe*",
                "cloudtrail:List*",
                "cloudtrail:LookupEvents"
            ],
            "Resource": "*"
        }
    ]
}
```

In the policy statements, the `Effect` element specifies whether the actions are allowed or denied\. The `Action` element lists the specific actions that the user is allowed to perform\. The `Resource` element lists the AWS resources the user is allowed to perform those actions on\. For policies that control access to CloudTrail actions, the `Resource` element is usually set to `*`, a wildcard that means "all resources\." 

The values in the `Action` element correspond to the APIs that the services support\. The actions are preceded by `cloudtrail:` to indicate that they refer to CloudTrail actions\. You can use the `*` wildcard character in the `Action` element , such as in the following examples: 
+ `"Action": ["cloudtrail:*Logging"]`

  This allows all CloudTrail actions that end with "Logging" \(`StartLogging`, `StopLogging`\)\.
+ `"Action": ["cloudtrail:*"]`

  This allows all CloudTrail actions, but not actions for other AWS services\.
+ `"Action": ["*"]`

  This allows all AWS actions\. This permission is suitable for a user who acts as an AWS administrator for your account\.

The read\-only policy doesn't grant user permission for the `CreateTrail`, `UpdateTrail`, `StartLogging`, and `StopLogging` actions\. Users with this policy are not allowed to create trails, update trails, or turn logging on and off\. For the list of CloudTrail actions, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

### Full access<a name="grant-custom-permissions-for-cloudtrail-users-full-access"></a>

The following example shows a policy that grants full access to CloudTrail\. This is equivalent to the managed policy **AWSCloudTrail\_FullAccess**\. It grants users the permission to perform all CloudTrail actions\. It also lets users log data events in Amazon S3 and AWS Lambda, manage files in Amazon S3 buckets, manage how CloudWatch Logs monitors CloudTrail log events, and manage Amazon SNS topics in the account that the user is associated with\. 

**Important**  
The **AWSCloudTrail\_FullAccess** policy or equivalent permissions are not intended to be shared broadly across your AWS account\. Users with this role or equivalent access have the ability to disable or reconfigure the most sensitive and important auditing functions in their AWS accounts\. For this reason, this policy should be applied only to account administrators, and use of this policy should be closely controlled and monitored\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sns:AddPermission",
                "sns:CreateTopic",
                "sns:SetTopicAttributes",
                "sns:GetTopicAttributes"
            ],
            "Resource": [
                "arn:aws:sns:*:*:aws-cloudtrail-logs*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "sns:ListTopics"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:PutBucketPolicy"
            ],
            "Resource": [
                "arn:aws:s3:::aws-cloudtrail-logs*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets",
                "s3:GetBucketLocation",
                "s3:GetBucketPolicy"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudtrail:*",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup"
            ],
            "Resource": [
                "arn:aws:logs:*:*:log-group:aws-cloudtrail-logs*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:ListRoles",
                "iam:GetRolePolicy",
                "iam:GetUser"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "cloudtrail.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:CreateKey",
                "kms:CreateAlias",
                "kms:ListKeys",
                "kms:ListAliases"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:ListFunctions"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:ListGlobalTables",
                "dynamodb:ListTables"
            ],
            "Resource": "*"
        }
    ]
}
```

### Granting permission to view AWS Config information on the CloudTrail console<a name="grant-aws-config-permissions-for-cloudtrail-users"></a>

You can view event information on the CloudTrail console, including resources that are related to that event\. For these resources, you can choose the AWS Config icon to view the timeline for that resource in the AWS Config console\. Attach this policy to your users to grant them read\-only AWS Config access\. The policy doesn't grant them permission to change settings in AWS Config\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "config:Get*",
            "config:Describe*",
            "config:List*"
        ],
        "Resource": "*"
    }]
}
```

For more information, see [Viewing resources referenced with AWS Config](view-cloudtrail-events-console.md#viewing-resources-config)\.

### Granting permission to view and configure Amazon CloudWatch Logs information on the CloudTrail console<a name="grant-cloudwatch-permissions-for-cloudtrail-users"></a>

You can view and configure delivery of events to CloudWatch Logs in the CloudTrail console if you have sufficient permissions\. These are permissions that may be beyond those granted for CloudTrail administrators\. Attach this policy to administrators who will configure and manage CloudTrail integration with CloudWatch Logs\. The policy doesn't grant them permissions in CloudTrail or in CloudWatch Logs directly, but instead grants the permissions required to create and configure the role CloudTrail will assume to successfully deliver events to your CloudWatch Logs group\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "iam:CreateRole",
            "iam:PutRolePolicy",
            "iam:AttachRolePolicy",
            "iam:ListRoles",
            "iam:GetRolePolicy",
            "iam:GetUser"
        ],
        "Resource": "*"
    }]
}
```

For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

### Additional information<a name="cloudtrail-notifications-more-info-2"></a>

To learn more about using IAM to give identities, such as users and roles, access to resources in your account, see [Getting started](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-set-up.html) and [Access management for AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\. 