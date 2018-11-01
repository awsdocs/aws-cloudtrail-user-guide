# Granting Custom Permissions for CloudTrail Users<a name="grant-custom-permissions-for-cloudtrail-users"></a>

CloudTrail policies grant permissions to users who work with CloudTrail\. If you need to grant different permissions to users, you can attach a CloudTrail policy to an IAM group or to a user\. You can edit the policy to include or exclude specific permissions\. You can also create your own custom policy\. Policies are JSON documents that define the actions a user is allowed to perform and the resources that the user is allowed to perform those actions on\. 

**Contents**
+ [Read\-only access](#grant-custom-permissions-for-cloudtrail-users-read-only)
+ [Full access](#grant-custom-permissions-for-cloudtrail-users-full-access)
+ [Controlling User Permissions for Actions on Specific Trails](#grant-custom-permissions-for-cloudtrail-users-resource-level)
+ [Granting Permission to View AWS Config Information on the CloudTrail Console](#grant-aws-config-permissions-for-cloudtrail-users)
+ [Additional Information](#cloudtrail-notifications-more-info-2)

## Read\-only access<a name="grant-custom-permissions-for-cloudtrail-users-read-only"></a>

The following example shows a policy that grants read\-only access to CloudTrail trails\. It grants users permission to see trail information, but not to create or update trails\. The policy also grants permission to read objects in Amazon S3 buckets, but not create or delete them\. 

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
        "s3:ListAllMyBuckets",
        "kms:ListAliases"
      ],
      "Resource": "*"
    }
  ]
}
```

In the policy statements, the `Effect` element specifies whether the actions are allowed or denied\. The `Action` element lists the specific actions that the user is allowed to perform\. The `Resource` element lists the AWS resources the user is allowed to perform those actions on\. For policies that control access to CloudTrail actions, the `Resource` element is always set to `*`, a wildcard that means "all resources\." 

The values in the `Action` element correspond to the APIs that the services support\. The actions are preceded by `cloudtrail:` to indicate that they refer to CloudTrail actions\. You can use the `*` wildcard character in the `Action` element , such as in the following examples: 
+ `"Action": ["cloudtrail:*Logging"]`

  This allows all CloudTrail actions that end with "Logging" \(`StartLogging`, `StopLogging`\)\.
+ `"Action": ["cloudtrail:*"]`

  This allows all CloudTrail actions, but not actions for other AWS services\.
+ `"Action": ["*"]`

  This allows all AWS actions\. This permission is suitable for a user who acts as an AWS administrator for your account\.

The read\-only policy doesn't grant user permission for the `CreateTrail`, `UpdateTrail`, `StartLogging`, and `StopLogging` actions\. Users with this policy are not allowed to create trails, update trails, or turn logging on and off\. For the list of CloudTrail actions, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

## Full access<a name="grant-custom-permissions-for-cloudtrail-users-full-access"></a>

The following example shows a policy that grants full access to CloudTrail\. It grants users the permission to perform all CloudTrail actions\. It also lets users manage files in Amazon S3 buckets, manage how CloudWatch Logs monitors CloudTrail log events, and manage Amazon SNS topics in the account that the user is associated with\. 

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
        "s3:GetObject",
        "s3:ListAllMyBuckets",
        "s3:PutBucketPolicy",
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
    }	
  ]
}
```

## Controlling User Permissions for Actions on Specific Trails<a name="grant-custom-permissions-for-cloudtrail-users-resource-level"></a>

You can use resource\-level permissions to control a user's ability to perform specific actions on CloudTrail trails\. 

For example, you don't want users of your company’s developer group to start or stop logging on a specific trail, but you want to grant them permission to perform the `DescribeTrails` and `GetTrailStatus` actions on the trail\. You want the users of the developer group to perform the `StartLogging` or `StopLogging` actions on trails that they create and manage\. 

You can create two policy statements and then attach them to the developer user group\.

In the first policy, you deny the `StartLogging` and `StopLogging` actions for the trail ARN that you specify\. In the following example, the trail ARN is `arn:aws:cloudtrail:us-east-2:111122223333:trail/Default`\.

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
                "arn:aws:cloudtrail:us-east-2:111122223333:trail/Default"
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
            "TrailARN": "arn:aws:cloudtrail:us-east-2:111122223333:trail/Default", 
            "IsMultiRegionTrail": false, 
            "S3BucketName": "myS3bucket ", 
            "HomeRegion": "us-east-2"
        }
    ]
}
```

The user then runs the `get-trail-status` command on the trail that you specified in the first policy\.

```
$ aws --profile devgroup cloudtrail get-trail-status --name Default
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
$ aws --profile devgroup cloudtrail stop-logging --name Default
```

The command returns an access denied exception:

```
A client error (AccessDeniedException) occurred when calling the StopLogging operation: Unknown
```

The user runs the `start-logging` command on the same trail\. 

```
$ aws --profile devgroup cloudtrail start-logging --name Default
```

The command returns an access denied exception:

```
A client error (AccessDeniedException) occurred when calling the StartLogging operation: Unknown
```

With resource level permissions, you can grant or deny access to specific trails in your account\. 

## Granting Permission to View AWS Config Information on the CloudTrail Console<a name="grant-aws-config-permissions-for-cloudtrail-users"></a>

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

## Additional Information<a name="cloudtrail-notifications-more-info-2"></a>

 To learn more about creating IAM users, groups, policies, and permissions, see [Creating Your First IAM User and Administrators Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/GSGHowToCreateAdminsGroup.html) and [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/PermissionsAndPolicies.html) in the *IAM User Guide*\. 