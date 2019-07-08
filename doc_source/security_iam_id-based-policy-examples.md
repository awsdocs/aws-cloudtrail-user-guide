# AWS CloudTrail Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify CloudTrail resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users, groups, or roles that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy Best Practices](#security_iam_service-with-iam-policy-best-practices)
+ [Example: Allowing and Denying Actions For a Specified Trail](#security_iam_id-based-policy-examples-allow-deny-for-specific-trail)
+ [Examples: Creating and Applying Policies for Actions on Specific Trails](#grant-custom-permissions-for-cloudtrail-users-resource-level)
+ [Granting Permissions for Using the CloudTrail Console](#security_iam_id-based-policy-examples-console)
+ [Allow Users to View Their Own Permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Granting Custom Permissions for CloudTrail Users](#grant-custom-permissions-for-cloudtrail-users)

## Policy Best Practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete CloudTrail resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get Started Using AWS Managed Policies** – To start using CloudTrail quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get Started Using Permissions With AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant Least Privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for Sensitive Operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using Multi\-Factor Authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use Policy Conditions for Extra Security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Example: Allowing and Denying Actions For a Specified Trail<a name="security_iam_id-based-policy-examples-allow-deny-for-specific-trail"></a>

The following example demonstrates a policy that allows users with this policy to view the status and configuration of a trail and start and stop logging for a trail named *My\-First\-Trail*\. This trail was created in the US East \(Ohio\) Region \(its home region\) in the AWS account with the ID *123456789012*\.

```
{
  "Version": "2012-10-17",
  "Statement": [
      {
          "Effect": "Allow",
          "Action": [
              "cloudtrail:StartLogging",
              "cloudtrail:StopLogging",
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

## Examples: Creating and Applying Policies for Actions on Specific Trails<a name="grant-custom-permissions-for-cloudtrail-users-resource-level"></a>

You can use permissions and policies to control a user's ability to perform specific actions on CloudTrail trails\. 

For example, you don't want users of your company’s developer group to start or stop logging on a specific trail, but you want to grant them permission to perform the `DescribeTrails` and `GetTrailStatus` actions on the trail\. You want the users of the developer group to perform the `StartLogging` or `StopLogging` actions on trails that they manage\. 

You can create two policy statements and then attach them to the developer group you create in IAM\. For more information about groups in IAM, see [IAM Groups](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html) in the *IAM User Guide*\.

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

The following CLI examples show that the developer group has been configured in an AWS CLI profile named `devgroup`\. First, a user of `devgroup` runs the `describe-trails` command\. 

```
$ aws --profile devgroup cloudtrail describe-trails
```

The command complete successfully:

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

The command complete successfully:

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

Next, a user of `devgroup` runs the `stop-logging` command on the same trail\. 

```
$ aws --profile devgroup cloudtrail stop-logging --name Example-Trail
```

The command returns an access denied exception:

```
A client error (AccessDeniedException) occurred when calling the StopLogging operation: Unknown
```

The user runs the `start-logging` command on the same trail\. 

```
$ aws --profile devgroup cloudtrail start-logging --name Example-Trail
```

The command returns an access denied exception:

```
A client error (AccessDeniedException) occurred when calling the StartLogging operation: Unknown 
```

## Granting Permissions for Using the CloudTrail Console<a name="security_iam_id-based-policy-examples-console"></a>

### Granting Permissions for CloudTrail Administration<a name="grant-permissions-for-cloudtrail-administration"></a>

To allow users to administer a CloudTrail trail, you must grant explicit permissions to IAM users to perform the actions associated with CloudTrail tasks\. For most scenarios, you can do this using an AWS managed policy that contains predefined permissions\.

**Note**  
The permissions you grant to users to perform CloudTrail administration tasks are not the same as the permissions that CloudTrail itself requires in order to deliver log files to Amazon S3 buckets or send notifications to Amazon SNS topics\. For more information about those permissions, see [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.  
If you configure integration with Amazon CloudWatch Logs, CloudTrail also requires a role that it can assume to deliver events to an Amazon CloudWatch Logs log group\. This will require additional permissions to create the role, as well as the role itself\. For more information, see [Granting Permission to View and Configure Amazon CloudWatch Logs Information on the CloudTrail Console](#grant-cloudwatch-permissions-for-cloudtrail-users) and [Sending Events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\.

A typical approach is to create an IAM group that has the appropriate permissions and then add individual IAM users to that group\. For example, you might create an IAM group for users who should have full access to CloudTrail actions, and a separate group for users who should be able to view trail information but not create or change trails\.

**To create an IAM group and users for CloudTrail access**

1.  Open the IAM console at [https://console\.aws\.amazon\.com/iam](https://console.aws.amazon.com/iam)\.

1. From the dashboard, choose **Groups** in the navigation pane, and then choose **Create New Group**\. 

1. Type a name, and then choose **Next Step**\. 

1. On the **Attach Policy** page, find and select one of the following policies for CloudTrail:
   +  **AWSCloudTrailFullAccess**\. This policy gives users in the group full access to CloudTrail actions\. These users have permissions to manage the Amazon S3 bucket, the log group for CloudWatch Logs, and an Amazon SNS topic for a trail\.
**Note**  
The **AWSCloudTrailFullAccess** policy is not intended to be shared broadly across your AWS account\. Users with this role have the ability to disable or reconfigure the most sensitive and important auditing functions in their AWS accounts\. For this reason, this policy should be applied only to account administrators, and use of this policy should be closely controlled and monitored\.
   +  **AWSCloudTrailReadOnlyAccess**\. This policy lets users in the group view the CloudTrail console, including recent events and event history\. These users can also view existing trails and their buckets\. Users can download a file of event history, but they cannot create or update trails\.
**Note**  
You can also create a custom policy that grants permissions to individual actions\. For more information, see [Granting Custom Permissions for CloudTrail Users](#grant-custom-permissions-for-cloudtrail-users)\.

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

#### Additional Resources<a name="cloudtrail-notifications-more-info-3"></a>

To learn more about creating IAM users, groups, policies, and permissions, see [Creating an Admins Group Using the Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/GSGHowToCreateAdminsGroup.html) and [Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. 

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.

## Allow Users to View Their Own Permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

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
               "Resource": [
                   "arn:aws:iam::*:user/${aws:username}"
               ]
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

## Granting Custom Permissions for CloudTrail Users<a name="grant-custom-permissions-for-cloudtrail-users"></a>

CloudTrail policies grant permissions to users who work with CloudTrail\. If you need to grant different permissions to users, you can attach a CloudTrail policy to an IAM group or to a user\. You can edit the policy to include or exclude specific permissions\. You can also create your own custom policy\. Policies are JSON documents that define the actions a user is allowed to perform and the resources that the user is allowed to perform those actions on\. For specific examples, see [Example: Allowing and Denying Actions For a Specified Trail](#security_iam_id-based-policy-examples-allow-deny-for-specific-trail) and [Examples: Creating and Applying Policies for Actions on Specific Trails](#grant-custom-permissions-for-cloudtrail-users-resource-level)\.

**Contents**
+ [Read\-only access](#grant-custom-permissions-for-cloudtrail-users-read-only)
+ [Full access](#grant-custom-permissions-for-cloudtrail-users-full-access)
+ [Granting Permission to View AWS Config Information on the CloudTrail Console](#grant-aws-config-permissions-for-cloudtrail-users)
+ [Granting Permission to View and Configure Amazon CloudWatch Logs Information on the CloudTrail Console](#grant-cloudwatch-permissions-for-cloudtrail-users)
+ [Additional Information](#cloudtrail-notifications-more-info-2)

### Read\-only access<a name="grant-custom-permissions-for-cloudtrail-users-read-only"></a>

The following example shows a policy that grants read\-only access to CloudTrail trails\. This is equivalent to the managed policy **AWSCloudTrailReadOnlyAccess**\. It grants users permission to see trail information, but not to create or update trails\. The policy also grants permission to read objects in Amazon S3 buckets, but not create or delete them\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:GetBucketLocation"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudtrail:DescribeTrails",
        "cloudtrail:GetTrailStatus",
        "cloudtrail:LookupEvents",
        "cloudtrail:ListPublicKeys",
        "cloudtrail:ListTags",
        "s3:ListAllMyBuckets",
        "kms:ListAliases",
        "lambda:ListFunctions"
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

The following example shows a policy that grants full access to CloudTrail\. This is equivalent to the managed policy **AWSCloudTrailFullAccess**\. It grants users the permission to perform all CloudTrail actions\. It also lets users log data events in Amazon S3 and AWS Lambda, manage files in Amazon S3 buckets, manage how CloudWatch Logs monitors CloudTrail log events, and manage Amazon SNS topics in the account that the user is associated with\. 

**Important**  
The **AWSCloudTrailFullAccess** policy or equivalent permissions are not intended to be shared broadly across your AWS account\. Users with this role or equivalent access have the ability to disable or reconfigure the most sensitive and important auditing functions in their AWS accounts\. For this reason, this policy should be applied only to account administrators, and use of this policy should be closely controlled and monitored\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sns:AddPermission",
                "sns:CreateTopic",
                "sns:DeleteTopic",
                "sns:ListTopics",
                "sns:SetTopicAttributes",
                "sns:GetTopicAttributes"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket",
                "s3:DeleteBucket",
                "s3:ListAllMyBuckets",
                "s3:PutBucketPolicy",
                "s3:ListBucket",
                "s3:GetObject",
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
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole",
                "iam:ListRoles",
                "iam:GetRolePolicy",
                "iam:GetUser"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
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
        }
    ]
}
```

### Granting Permission to View AWS Config Information on the CloudTrail Console<a name="grant-aws-config-permissions-for-cloudtrail-users"></a>

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

For more information, see [Viewing Resources Referenced with AWS Config](view-cloudtrail-events-console.md#viewing-resources-config)\.

### Granting Permission to View and Configure Amazon CloudWatch Logs Information on the CloudTrail Console<a name="grant-cloudwatch-permissions-for-cloudtrail-users"></a>

You can view and configure delivery of events to CloudWatch Logs in the CloudTrail console if you have sufficient permissions\. These are permissions that may be beyond those granted for CloudTrail administrators\. Attach this policy to administrators who will configure and manage CloudTrail integration with CloudWatch Logs\. The policy doesn't grant them permissions in CloudTrail or in CloudWatch Logs directly, but instead grants the permissions required to create and configure the role CloudTrail will assume to successfully deliver events to your CloudWatch Logs group\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "iam:CreateRole"
            "iam:PutRolePolicy"
            "iam:ListRoles"
            "iam:GetRolePolicy"
            "iam:GetUser"
        ],
        "Resource": "*"
    }]
}
```

For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

### Additional Information<a name="cloudtrail-notifications-more-info-2"></a>

 To learn more about creating IAM users, groups, policies, and permissions, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/GSGHowToCreateAdminsGroup.html) and [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. 