# AWS managed policies for AWS CloudTrail<a name="security-iam-awsmanpol"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

## AWS managed policy: `AWSCloudTrail_ReadOnlyAccess`<a name="security-iam-awsmanpol-AWSCloudTrail-ReadOnlyAccess"></a>

A user identity that has the `AWSCloudTrail_ReadOnlyAccess` policy attached to its role can perform read\-only actions in CloudTrail, such as `Get*`, `List*`, and `Describe*` actions on trails, CloudTrail Lake event data stores, or Lake queries\.

**Permissions details**

This policy includes the following permissions\.
+ `cloudtrail` – Allows user identities to perform read\-only API operations in the CloudTrail console, the AWS SDKs, and the AWS CLI, including `Get*` operations such as `GetEventDataStores`, `List*` operations such as `ListTrails`, and `Describe*` operations such as `DescribeTrails`\.

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

## CloudTrail updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>

View details about updates to AWS managed policies for CloudTrail\. For automatic alerts about changes to this page, subscribe to the RSS feed on the CloudTrail [Document history](cloudtrail-document-history.md) page\.


| Change | Description | Date | 
| --- | --- | --- | 
|  [`AWSCloudTrail_ReadOnlyAccess`](#security-iam-awsmanpol-AWSCloudTrail-ReadOnlyAccess) – Update to an existing policy  |  CloudTrail changed the name of the `AWSCloudTrailReadOnlyAccess` policy to `AWSCloudTrail_ReadOnlyAccess`\. Also, the scope of permissions in the policy has been reduced to CloudTrail actions\. It no longer includes Amazon S3, AWS KMS, or AWS Lambda action permissions\.  | June 6, 2022 | 
|  CloudTrail started tracking changes  |  CloudTrail started tracking changes for its AWS managed policies\.  | June 6, 2022 | 