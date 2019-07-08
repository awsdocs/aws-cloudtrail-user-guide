# Amazon SNS Topic Policy for CloudTrail<a name="cloudtrail-permissions-for-sns-notifications"></a>

To send notifications to an SNS topic, CloudTrail must have the required permissions\. CloudTrail automatically attaches the required permissions to the topic when you create an Amazon SNS topic as part of creating or updating a trail in the CloudTrail console\.

CloudTrail adds the following statement to the policy for you with the following fields:
+ The allowed SIDs\.
+ The service principal name for CloudTrail\.
+ The SNS topic, including region, account ID, and topic name\.

The following policy allows CloudTrail to send notifications about log file delivery from supported regions\. For more information, see [CloudTrail Supported Regions](cloudtrail-supported-regions.md)\. 

**SNS topic policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailSNSPolicy20131101",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": "SNS:Publish",
            "Resource": "arn:aws:sns:region:SNSTopicOwnerAccountId:SNSTopicName"
        }
    ]
}
```<a name="cmk-policy"></a>

If you want to use a AWS KMS\-encrypted Amazon SNS topic to send notifications, you must also enable compatibility between the event source \(CloudTrail\) and the encrypted topic by adding the following statement to the policy of the customer master key \(CMK\)\.

**CMK policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudtrail.amazonaws.com"
            },
            "Action": [
                "kms:GenerateDataKey*",
                "kms:Decrypt"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information, see [Enable Compatibility between Event Sources from AWS Services and Encrypted Topics](https://docs.aws.amazon.com/sns/latest/dg/sns-server-side-encryption.html#sns-what-permissions-for-sse)\.

**Contents**
+ [Specifying an Existing Topic for Sending Notifications](#specifying-an-existing-topic-for-sns-notifications)
+ [Troubleshooting the SNS Topic Policy](#troubleshooting-sns-topic-policy)
  + [Common SNS Policy Configuration Errors](#sns-topic-policy-for-multiple-regions)
+ [Additional Resources](#cloudtrail-notifications-more-info-5)

## Specifying an Existing Topic for Sending Notifications<a name="specifying-an-existing-topic-for-sns-notifications"></a>

You can manually add the permissions for an Amazon SNS topic to your topic policy in the Amazon SNS console and then specify the topic in the CloudTrail console\.

**To manually update an SNS topic policy**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

1. Choose **Topics** and then choose the topic\.

1. Choose **Other topic actions** and then choose **Edit topic policy**\.

1. Choose **Advanced view**, and add the statement from [SNS topic policy](#sns-topic-policy) with the appropriate values for the region, account ID, and topic name\.

1. Choose **Update policy**\.

1. If your topic is an encrypted topic, you must allow CloudTrail to have the `kms:GenerateDataKey*` and the `kms:Decrypt` permissions\. For more information, see [Encrypted SNS topic CMK policy](#cmk-policy)\. 

1. Return to the CloudTrail console and specify the topic for the trail\.

## Troubleshooting the SNS Topic Policy<a name="troubleshooting-sns-topic-policy"></a>

The following sections describe how to troubleshoot the SNS topic policy\.

### Common SNS Policy Configuration Errors<a name="sns-topic-policy-for-multiple-regions"></a>

When you create a new topic as part of creating or updating a trail, CloudTrail attaches the required permissions to your topic\. The topic policy uses the service principal name, `"cloudtrail.amazonaws.com"`, which allows CloudTrail to send notifications for all regions\.

If CloudTrail is not sending notifications for a region, it's possible that your topic has an older policy that specifies CloudTrail account IDs for each region\. This policy gives CloudTrail permission to send notifications only for the regions specified\.

The following topic policy allows CloudTrail to send notifications for the specified nine regions only:

**Example topic policy with account IDs**  

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "AWSCloudTrailSNSPolicy20131101",
        "Effect": "Allow",
        "Principal": {"AWS": [
            "arn:aws:iam::903692715234:root",
            "arn:aws:iam::035351147821:root",
            "arn:aws:iam::859597730677:root",
            "arn:aws:iam::814480443879:root",
            "arn:aws:iam::216624486486:root",
            "arn:aws:iam::086441151436:root",
            "arn:aws:iam::388731089494:root",
            "arn:aws:iam::284668455005:root",
            "arn:aws:iam::113285607260:root"
        ]},
        "Action": "SNS:Publish",
        "Resource": "aws:arn:sns:us-east-1:123456789012:myTopic"
    }]
}
```

This policy uses a permission based on individual CloudTrail account IDs\. To deliver logs for a new region, you must manually update the policy to include the CloudTrail account ID for that region\. For example, because CloudTrail added support for the US East \(Ohio\) Region, you must update the policy to add the account ID ARN for that region: `"arn:aws:iam::475085895292:root"`\.

As a best practice, update the policy to use a permission with the CloudTrail service principal\. To do this, replace the account ID ARNs with the service principal name: `"cloudtrail.amazonaws.com"`\.

This gives CloudTrail permission to send notifications for current and new regions\. The following is an updated version of the previous policy:

**Example topic policy with service principal name**  

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "AWSCloudTrailSNSPolicy20131101",
        "Effect": "Allow",
        "Principal": {Service": "cloudtrail.amazonaws.com"},
        "Action": "SNS:Publish",
        "Resource": arn:aws:sns:us-east-1:123456789012:myTopic"
    }]
}
```

Verify that the policy has the correct values:
+ In the `Resource` field, specify the account number of the topic owner\. For topics that you create, specify your account number\.
+ Specify the appropriate values for the region and SNS topic name\.

## Additional Resources<a name="cloudtrail-notifications-more-info-5"></a>

For more information about SNS topics and subscribing to them, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/)\.