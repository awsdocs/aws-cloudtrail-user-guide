# Amazon S3 bucket policy for CloudTrail<a name="create-s3-bucket-policy-for-cloudtrail"></a>

By default, Amazon S3 buckets and objects are private\. Only the resource owner \(the AWS account that created the bucket\) can access the bucket and objects it contains\. The resource owner can grant access permissions to other resources and users by writing an access policy\.

To create or modify an Amazon S3 bucket to receive log files for an organization trail, you must change the bucket policy\. For more information, see [Creating a trail for an organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.

To deliver log files to an S3 bucket, CloudTrail must have the required permissions, and it cannot be configured as a [Requester Pays](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html) bucket\.

CloudTrail adds the following fields in the policy for you: 
+ The allowed SIDs
+ The bucket name
+ The service principal name for CloudTrail
+ The name of the folder where the log files are stored, including the bucket name, a prefix \(if you specified one\), and your AWS account ID

As a security best practice, add an `aws:SourceArn` condition key to the Amazon S3 bucket policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail writes to the S3 bucket only for a specific trail or trails\. The value of `aws:SourceArn` is always the ARN of the trail \(or array of trail ARNs\) that is using the bucket to store logs\. Be sure to add the `aws:SourceArn` condition key to S3 bucket policies for existing trails\.

The following policy allows CloudTrail to write log files to the bucket from supported regions\. Replace *myBucketName*, *\[optionalPrefix\]/*, *myAccountID*, *region*, and *trailName* with the appropriate values for your configuration\.

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
            "Resource": "arn:aws:s3:::myBucketName",
            "Condition": {
                "StringEquals": {
                    "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:trail/trailName"
                }
            }
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::myBucketName/[optionalPrefix]/AWSLogs/myAccountID/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control",
                    "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:trail/trailName"
                }
            }
        }
    ]
}
```

For more information about AWS Regions, see [CloudTrail supported Regions](cloudtrail-supported-regions.md)\.

**Contents**
+ [Specifying an existing bucket for CloudTrail log delivery](#specify-an-existing-bucket-for-cloudtrail-log-delivery)
+ [Receiving log files from other accounts](#aggregration-option)
+ [Create or update an Amazon S3 bucket to use to store the log files for an organization trail](#org-trail-bucket-policy)
+ [Troubleshooting the Amazon S3 bucket policy](#troubleshooting-s3-bucket-policy)
  + [Common Amazon S3 policy configuration errors](#s3-bucket-policy-for-multiple-regions)
  + [Changing a prefix for an existing bucket](#cloudtrail-add-change-or-remove-a-bucket-prefix)
+ [Additional resources](#cloudtrail-S3-bucket-policy-resources)

## Specifying an existing bucket for CloudTrail log delivery<a name="specify-an-existing-bucket-for-cloudtrail-log-delivery"></a>

If you specified an existing S3 bucket as the storage location for log file delivery, you must attach a policy to the bucket that allows CloudTrail to write to the bucket\. 

**Note**  
As a best practice, use a dedicated S3 bucket for CloudTrail logs\.

**To add the required CloudTrail policy to an Amazon S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket where you want CloudTrail to deliver your log files, and then choose **Permissions**\. 

1. Choose **Edit**\.

1. Copy the [S3 bucket policy](#s3-bucket-policy) to the **Bucket Policy Editor** window\. Replace the placeholders in italics with the names of your bucket, prefix, and account number\. If you specified a prefix when you created your trail, include it here\. The prefix is an optional addition to the S3 object key that creates a folder\-like organization in your bucket\. 
**Note**  
If the existing bucket already has one or more policies attached, add the statements for CloudTrail access to that policy or policies\. Evaluate the resulting set of permissions to be sure that they are appropriate for the users who will access the bucket\. 

## Receiving log files from other accounts<a name="aggregration-option"></a>

You can configure CloudTrail to deliver log files from multiple AWS accounts to a single S3 bucket\. For more information, see [Receiving CloudTrail log files from multiple accountsRedacting bucket owner account IDs for data events called by other accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.

## Create or update an Amazon S3 bucket to use to store the log files for an organization trail<a name="org-trail-bucket-policy"></a>

You must specify an Amazon S3 bucket to receive the log files for an organization trail\. This bucket must have a policy that allows CloudTrail to put the log files for the organization into the bucket\.

The following is an example policy for an Amazon S3 bucket named *myOrganizationBucket*\. This bucket is in an AWS account with the ID *111111111111*, which is the management account for an organization with the ID *o\-exampleorgid* that allows logging for an organization trail\. It also allows logging for the *111111111111* account in the event that the trail is changed from an organization trail to a trail for that account only\. Replace *myOrganizationBucket*, *111111111111*, *region*, and *trailName* with the appropriate values for your configuration\.

The example policy includes an `aws:SourceArn` condition key for the Amazon S3 bucket policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail writes to the S3 bucket only for a specific trail or trails\. In an organization trail, the value of `aws:SourceArn` must be a trail ARN that is owned by the management account, and uses the management account ID\.

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
            "Resource": "arn:aws:s3:::myOrganizationBucket",
            "Condition": {
                "StringEquals": {
                    "aws:SourceArn": "arn:aws:cloudtrail:region:111111111111:trail/trailName"
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
            "Resource": "arn:aws:s3:::myOrganizationBucket/AWSLogs/111111111111/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control",
                    "aws:SourceArn": "arn:aws:cloudtrail:region:111111111111:trail/trailName"
                }
            }
        },
        {
            "Sid": "AWSCloudTrailOrganizationWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::myOrganizationBucket/AWSLogs/o-exampleorgid/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control",
                    "aws:SourceArn": "arn:aws:cloudtrail:region:111111111111:trail/trailName"
                }
            }
        }
    ]
}
```

This example policy does not allow any users from member accounts to access the log files created for the organization\. By default, organization log files are accessible only to the management account\. For information about how to allow read access to the Amazon S3 bucket for IAM users in member accounts, see [Sharing CloudTrail log files between AWS accounts](cloudtrail-sharing-logs.md)\.

## Troubleshooting the Amazon S3 bucket policy<a name="troubleshooting-s3-bucket-policy"></a>

The following sections describe how to troubleshoot the S3 bucket policy\.

### Common Amazon S3 policy configuration errors<a name="s3-bucket-policy-for-multiple-regions"></a>

When you create a new bucket as part of creating or updating a trail, CloudTrail attaches the required permissions to your bucket\. The bucket policy uses the service principal name, `"cloudtrail.amazonaws.com"`, which allows CloudTrail to deliver logs for all regions\.

If CloudTrail is not delivering logs for a region, it's possible that your bucket has an older policy that specifies CloudTrail account IDs for each region\. This policy gives CloudTrail permission to deliver logs only for the regions specified\.

As a best practice, update the policy to use a permission with the CloudTrail service principal\. To do this, replace the account ID ARNs with the service principal name: `"cloudtrail.amazonaws.com"`\. This gives CloudTrail permission to deliver logs for current and new regions\. As a security best practice, add an `aws:SourceArn` or `aws:SourceAccount` condition key to the Amazon S3 bucket policy\. This helps prevent unauthorized account access to your S3 bucket\. If you have existing trails, be sure to add one or more condition keys\. The following example shows a recommended policy configuration\. Replace *myBucketName*, *\[optionalPrefix\]/*, *myAccountID*, *region*, and *trailName* with the appropriate values for your configuration\.

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
            "Resource": "arn:aws:s3:::myBucketName",
            "Condition": {
                "StringEquals": {
                    "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:trail/trailName"
                }
            }
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::myBucketName/[optionalPrefix]/AWSLogs/myAccountID/*",
            "Condition": {"StringEquals": {
                "s3:x-amz-acl": "bucket-owner-full-control",
                "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:trail/trailName"
                }
            }
        }
    ]
}
```

### Changing a prefix for an existing bucket<a name="cloudtrail-add-change-or-remove-a-bucket-prefix"></a>

If you try to add, modify, or remove a log file prefix for an S3 bucket that receives logs from a trail, you might see the error: **There is a problem with the bucket policy**\. A bucket policy with an incorrect prefix can prevent your trail from delivering logs to the bucket\. To resolve this issue, use the Amazon S3 console to update the prefix in the bucket policy, and then use the CloudTrail console to specify the same prefix for the bucket in the trail\. 

**To update the log file prefix for an Amazon S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket for which you want to modify the prefix, and then choose **Permissions**\. 

1. Choose **Edit**\.

1. In the bucket policy, under the `s3:PutObject` action, edit the `Resource` entry to add, modify, or remove the log file *prefix/* as needed\.

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

## Additional resources<a name="cloudtrail-S3-bucket-policy-resources"></a>

For more information about S3 buckets and policies, see the [Amazon Simple Storage Service User Guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/)\.