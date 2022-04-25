# Amazon SNS topic policy for CloudTrail<a name="cloudtrail-permissions-for-sns-notifications"></a>

To send notifications to an SNS topic, CloudTrail must have the required permissions\. CloudTrail automatically attaches the required permissions to the topic when you create an Amazon SNS topic as part of creating or updating a trail in the CloudTrail console\.

**Important**  
As a security best practice, to restrict access to your SNS topic, we strongly recommend that after you create or update a trail to send SNS notifications, you manually edit the IAM policy that is attached to the SNS topic to add condition keys\. For more information, see [Security best practice for SNS topic policy](#cloudtrail-sns-notifications-policy-security) in this topic\.

CloudTrail adds the following statement to the policy for you with the following fields:
+ The allowed SIDs\.
+ The service principal name for CloudTrail\.
+ The SNS topic, including region, account ID, and topic name\.

The following policy allows CloudTrail to send notifications about log file delivery from supported regions\. For more information, see [CloudTrail supported regions](cloudtrail-supported-regions.md)\. This is the default policy that is attached to a new or existing SNS topic policy when you create or update a trail, and choose to enable SNS notifications\.

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
```<a name="kms-key-policy"></a>

To use an AWS KMS\-encrypted Amazon SNS topic to send notifications, you must also enable compatibility between the event source \(CloudTrail\) and the encrypted topic by adding the following statement to the policy of the AWS KMS key\.

**KMS key policy**

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
+ [Security best practice for SNS topic policy](#cloudtrail-sns-notifications-policy-security)
+ [Specifying an existing topic for sending notifications](#specifying-an-existing-topic-for-sns-notifications)
+ [Troubleshooting the SNS topic policy](#troubleshooting-sns-topic-policy)
  + [Common SNS policy configuration errors](#sns-topic-policy-for-multiple-regions)
+ [Additional resources](#cloudtrail-notifications-more-info-5)

## Security best practice for SNS topic policy<a name="cloudtrail-sns-notifications-policy-security"></a>

By default, the IAM policy statement that CloudTrail attaches to your Amazon SNS topic allows the CloudTrail service principal to publish to an SNS topic, identified by an ARN\. To help prevent an attacker from gaining access to your SNS topic, and sending notifications on behalf of CloudTrail to topic recipients, manually edit your CloudTrail SNS topic policy to add an `aws:SourceArn` condition key to the policy statement attached by CloudTrail\. The value of this key is the ARN of the trail, or an array of trail ARNs that are using the SNS topic\. Because it includes both the specific trail ID and the ID of the account that owns the trail, it restricts SNS topic access to only those accounts that have permission to manage the trail\. Before you add condition keys to your SNS topic policy, get the SNS topic name from your trail's settings in the CloudTrail console\.

The `aws:SourceAccount` condition key is also supported, but is not recommended\.

**To add the `aws:SourceArn` condition key to your SNS topic policy**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics**\.

1. Choose the SNS topic that is shown in your trail settings, and then choose **Edit**\.

1. Expand **Access policy**\.

1. In the **Access policy** JSON editor, look for a block that resembles the following example\.

   ```
       {
         "Sid": "AWSCloudTrailSNSPolicy20150319",
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudtrail.amazonaws.com"
         },
         "Action": "SNS:Publish",
         "Resource": "arn:aws:sns:us-west-2:111122223333:aws-cloudtrail-logs-111122223333-61bbe496"
       }
   ```

1. Add a new block for a condition, `aws:SourceArn`, as shown in the following example\. The value of `aws:SourceArn` is the ARN of the trail about which you are sending notifications to SNS\.

   ```
       {
         "Sid": "AWSCloudTrailSNSPolicy20150319",
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudtrail.amazonaws.com"
         },
         "Action": "SNS:Publish",
         "Resource": "arn:aws:sns:us-west-2:111122223333:aws-cloudtrail-logs-111122223333-61bbe496",
         "Condition": {
           "StringEquals": {
             "aws:SourceArn": "arn:aws:cloudtrail:us-west-2:123456789012:trail/Trail3"
           }
         }
       }
   ```

1. When you are finished editing the SNS topic policy, choose **Save changes**\.

**To add the `aws:SourceAccount` condition key to your SNS topic policy**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics**\.

1. Choose the SNS topic that is shown in your trail settings, and then choose **Edit**\.

1. Expand **Access policy**\.

1. In the **Access policy** JSON editor, look for a block that resembles the following example\.

   ```
       {
         "Sid": "AWSCloudTrailSNSPolicy20150319",
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudtrail.amazonaws.com"
         },
         "Action": "SNS:Publish",
         "Resource": "arn:aws:sns:us-west-2:111122223333:aws-cloudtrail-logs-111122223333-61bbe496"
       }
   ```

1. Add a new block for a condition, `aws:SourceAccount`, as shown in the following example\. The value of `aws:SourceAccount` is the ID of the account that owns the CloudTrail trail\. This example restricts access to the SNS topic to only those users who can sign in to the AWS account 123456789012\.

   ```
       {
         "Sid": "AWSCloudTrailSNSPolicy20150319",
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudtrail.amazonaws.com"
         },
         "Action": "SNS:Publish",
         "Resource": "arn:aws:sns:us-west-2:111122223333:aws-cloudtrail-logs-111122223333-61bbe496",
         "Condition": {
           "StringEquals": {
             "aws:SourceAccount": "123456789012"
           }
         }
       }
   ```

1. When you are finished editing the SNS topic policy, choose **Save changes**\.

## Specifying an existing topic for sending notifications<a name="specifying-an-existing-topic-for-sns-notifications"></a>

You can manually add the permissions for an Amazon SNS topic to your topic policy in the Amazon SNS console and then specify the topic in the CloudTrail console\.

**To manually update an SNS topic policy**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. Choose **Topics** and then choose the topic\.

1. Choose **Other topic actions** and then choose **Edit topic policy**\.

1. Choose **Advanced view**, and add the statement from [SNS topic policy](#sns-topic-policy) with the appropriate values for the region, account ID, and topic name\.

1. Choose **Update policy**\.

1. If your topic is an encrypted topic, you must allow CloudTrail to have `kms:GenerateDataKey*` and the `kms:Decrypt` permissions\. For more information, see [Encrypted SNS topic KMS key policy](#kms-key-policy)\.

1. Return to the CloudTrail console and specify the topic for the trail\.

## Troubleshooting the SNS topic policy<a name="troubleshooting-sns-topic-policy"></a>

The following sections describe how to troubleshoot the SNS topic policy\.

### Common SNS policy configuration errors<a name="sns-topic-policy-for-multiple-regions"></a>

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
        "Principal": {"Service": "cloudtrail.amazonaws.com"},
        "Action": "SNS:Publish",
        "Resource": "arn:aws:sns:us-west-2:123456789012:myTopic"
    }]
}
```

Verify that the policy has the correct values:
+ In the `Resource` field, specify the account number of the topic owner\. For topics that you create, specify your account number\.
+ Specify the appropriate values for the region and SNS topic name\.

## Additional resources<a name="cloudtrail-notifications-more-info-5"></a>

For more information about SNS topics and subscribing to them, see the [Amazon Simple Notification Service Developer Guide](https://docs.aws.amazon.com/sns/latest/dg/)\.