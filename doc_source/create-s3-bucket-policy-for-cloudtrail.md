# Amazon S3 Bucket Policy for CloudTrail<a name="create-s3-bucket-policy-for-cloudtrail"></a>

By default, Amazon S3 buckets and objects are private\. Only the resource owner \(the AWS account that created the bucket\) can access the bucket and objects it contains\. The resource owner can grant access permissions to other resources and users by writing an access policy\. 

If you want to create or modify an Amazon S3 bucket to receive the log files for an organization trail, you must further modify the bucket policy\. For more information, see [Creating a Trail for an Organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.

To deliver log files to an S3 bucket, CloudTrail must have the required permissions, and it cannot be configured as a [Requester Pays](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html) bucket\. CloudTrail automatically attaches the required permissions to a bucket when you create an Amazon S3 bucket as part of creating or updating a trail in the CloudTrail console\.

 CloudTrail adds the following fields in the policy for you: 
+ The allowed SIDs\.
+ The bucket name\.
+ The service principal name for CloudTrail\.
+ The name of the folder where the log files are stored, including the bucket name, a prefix \(if you specified one\), and your AWS account ID\.

The following policy allows CloudTrail to write log files to the bucket from supported regions\. For more information, see [CloudTrail Supported Regions](cloudtrail-supported-regions.md)\. 

**S3 bucket policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck20150319",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::myBucketName"
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::myBucketName/[optional prefix]/AWSLogs/myAccountID/*",
            "Condition": {"StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}}
        }
    ]
}
```

**Contents**
+ [Specifying an Existing Bucket for CloudTrail Log Delivery](#specify-an-existing-bucket-for-cloudtrail-log-delivery)
+ [Receiving Log Files from Other Accounts](#aggregration-option)
+ [Create or update an Amazon S3 bucket to use to store the log files for an organization trail](#w61aac16c21c27c41)
+ [Troubleshooting the S3 Bucket Policy](#troubleshooting-s3-bucket-policy)
  + [Common S3 Policy Configuration Errors](#s3-bucket-policy-for-multiple-regions)
  + [Changing a Prefix for an Existing Bucket](#cloudtrail-add-change-or-remove-a-bucket-prefix)
+ [Additional Resources](#cloudtrail-S3-bucket-policy-resources)

## Specifying an Existing Bucket for CloudTrail Log Delivery<a name="specify-an-existing-bucket-for-cloudtrail-log-delivery"></a>

If you specified an existing S3 bucket as the storage location for log file delivery, you must attach a policy to the bucket that allows CloudTrail to write to the bucket\. 

**Note**  
As a best practice, use a dedicated S3 bucket for CloudTrail logs\.

**To add the required CloudTrail policy to an Amazon S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket where you want CloudTrail to deliver your log files, and then choose **Properties**\. 

1. Choose **Permissions**\.

1. Choose **Edit Bucket Policy**\.

1. Copy the [S3 bucket policy](#s3-bucket-policy) to the **Bucket Policy Editor** window\. Replace the placeholders in italics with the names of your bucket, prefix, and account number\. If you specified a prefix when you created your trail, include it here\. The prefix is an optional addition to the S3 object key that creates a folder\-like organization in your bucket\. 
**Note**  
If the existing bucket already has one or more policies attached, add the statements for CloudTrail access to that policy or policies\. Evaluate the resulting set of permissions to be sure that they are appropriate for the users who will access the bucket\. 

## Receiving Log Files from Other Accounts<a name="aggregration-option"></a>

You can configure CloudTrail to deliver log files from multiple AWS accounts to a single S3 bucket\. For more information, see [Receiving CloudTrail Log Files from Multiple Accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.

## Create or update an Amazon S3 bucket to use to store the log files for an organization trail<a name="w61aac16c21c27c41"></a>

You must specify an Amazon S3 bucket to receive the log files for an organization trail\. This bucket must have a policy that allows CloudTrail to put the log files for the organization into the bucket\.

The following is an example policy for an Amazon S3 bucket named *my\-organization\-bucket*\. This bucket is in an AWS account with the ID *111111111111*, which is the master account for an organization with the ID *o\-exampleorgid* that allows logging for an organization trail\. It also allows logging for the *111111111111* account in the event that the trail is changed from an organization trail to a trail for that account only\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::my-organization-bucket"
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::my-organization-bucket/AWSLogs/111111111111/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::my-organization-bucket/AWSLogs/o-exampleorgid/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        }
    ]
}
```

This example policy does not allow any users from member accounts to access the log files created for the organization\. By default, organization log files are accessible only to the master account\. For information about how to allow read access to the Amazon S3 bucket for IAM users in member accounts, see [Sharing CloudTrail Log Files Between AWS Accounts](cloudtrail-sharing-logs.md)\.

## Troubleshooting the S3 Bucket Policy<a name="troubleshooting-s3-bucket-policy"></a>

The following sections describe how to troubleshoot the S3 bucket policy\.

### Common S3 Policy Configuration Errors<a name="s3-bucket-policy-for-multiple-regions"></a>

When you create a new bucket as part of creating or updating a trail, CloudTrail attaches the required permissions to your bucket\. The bucket policy uses the service principal name, `"cloudtrail.amazonaws.com"`, which allows CloudTrail to deliver logs for all regions\.

If CloudTrail is not delivering logs for a region, it's possible that your bucket has an older policy that specifies CloudTrail account IDs for each region\. This policy gives CloudTrail permission to deliver logs only for the regions specified\.

The following bucket policy allows CloudTrail to deliver logs for the specified nine regions only:

**Example bucket policy with account IDs**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck20131101",
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
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::bucket-1"
        },
        {
            "Sid": "AWSCloudTrailWrite20131101",
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
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::bucket-1/my-prefix/AWSLogs/123456789012/*",
            "Condition": {"StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}}
        }
    ]
}
```

This policy uses a permission based on individual CloudTrail account IDs\. To send notifications for a new region, you must manually update the policy to include the CloudTrail account ID for that region\. For example, because CloudTrail added support for the US East \(Ohio\) Region, you must update the policy to include the account ID ARN for that region: `"arn:aws:iam::475085895292:root`"\. 

As a best practice, update the policy to use a permission with the CloudTrail service principal\. To do this, replace the account ID ARNs with the service principal name: `"cloudtrail.amazonaws.com"`\. This gives CloudTrail permission to deliver logs for current and new regions\. The following is an updated version of the previous policy:

**Example bucket policy with service principal name**  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck20150319",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::bucket-1"
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::bucket-1/my-prefix/AWSLogs/123456789012/*",
            "Condition": {"StringEquals": {"s3:x-amz-acl": "bucket-owner-full-control"}}
        }
    ]
}
```

### Changing a Prefix for an Existing Bucket<a name="cloudtrail-add-change-or-remove-a-bucket-prefix"></a>

If you try to add, modify, or remove a log file prefix for an S3 bucket that receives logs from a trail, you may see the error: **There is a problem with the bucket policy**\. A bucket policy with an incorrect prefix can prevent your trail from delivering logs to the bucket\. To resolve this issue, use the Amazon S3 console to update the prefix in the bucket policy, and then use the CloudTrail console to specify the same prefix for the bucket in the trail\. 

**To update the log file prefix for an S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket for which you want to modify the prefix, and then choose **Properties**\. 

1. Choose **Permissions**\.

1. Choose **Edit Bucket Policy**\.

1. In the bucket policy, under the `s3:PutObject` action, edit the `Resource` entry to add, modify, or remove the log file *prefix* as needed\.

   ```
   "Action": "s3:PutObject",
         "Resource": "arn:aws:s3:::myBucketName/prefix/AWSLogs/myAccountID/*",
   ```

1. Choose **Save**\.

1. Open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose your trail and for **Storage location**, click the pencil icon to edit the settings for your bucket\.

1. For **S3 bucket**, choose the bucket with the prefix you are changing\.

1. For **Log file prefix**, update the prefix to match the prefix that you entered in the bucket policy\.

1. Choose **Save**\.

## Additional Resources<a name="cloudtrail-S3-bucket-policy-resources"></a>

For more information about S3 buckets and policies, see the [Amazon Simple Storage Service Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/)\.