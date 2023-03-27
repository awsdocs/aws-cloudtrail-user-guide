# Copying trail events to CloudTrail Lake<a name="cloudtrail-copy-trail-to-lake"></a>

You can copy existing trail events to a CloudTrail Lake event data store to create a point\-in\-time snapshot of events logged to the trail\. Copying trail events does not interfere with the trail's ability to log events and does not modify the trail in any way\.

If you are copying trail events to an organization event data store, you must use the management account for the organization\. You cannot copy trail events using the delegated administrator account for an organization\.

Copying trail events to a CloudTrail Lake event data store, allows you to run queries on the copied events\. CloudTrail Lake queries offer a deeper and more customizable view of events than simple key and value lookups in Event history, or running `LookupEvents`\. For more information on CloudTrail Lake, see [Working with AWS CloudTrail Lake](cloudtrail-lake.md)\.

Before you copy trail events to CloudTrail Lake, create or have an event data store ready\.

**Note**  
 After copying trail events to Lake, you might want to turn off logging for the trail to avoid additional charges\. For more information, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. 

**Topics**
+ [Considerations](#cloudtrail-trail-copy-considerations)
+ [Required permissions for copying trail events](#cloudtrail-copy-trail-events-permissions)
+ [Copy trail events to CloudTrail Lake using the CloudTrail console](#cloudtrail-copy-trail-events)

## Considerations<a name="cloudtrail-trail-copy-considerations"></a>

Consider the following factors when copying trail events\.
+  When you copy trail events to an event data store, CloudTrail copies all trail events regardless of the configuration of the destination event data store's event types, advanced event selectors, or region\. 
+  Before copying trail events, check the retention period of the event data store\. CloudTrail only copies trail events that are within the event data store’s retention period\. For example, if an event data store’s retention period is 90 days, then CloudTrail will not copy any trail events older than 90 days\. 
+  Before copying trail events, disable any access control lists \(ACLs\) attached to the source S3 bucket, and update the S3 bucket policy for the destination event data store\. For more information about updating the S3 bucket policy, see [Amazon S3 bucket policy for copying trail events](#cloudtrail-copy-trail-events-permissions-s3)\. For more information about disabling ACLs, see [ Controlling ownership of objects and disabling ACLs for your bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/about-object-ownership.html)\. 
+  CloudTrail only copies trail events from Gzip compressed log files that are in the source S3 bucket\. CloudTrail does not copy trail events from uncompressed log files, or log files that were compressed using a format other than Gzip\. 
+  To avoid duplicating events between the source trail and destination event data store, choose a time range for the copied events that is earlier than the creation of the event data store\. 
+  By default, CloudTrail only copies CloudTrail events contained in the S3 bucket's `CloudTrail` prefix and the prefixes inside the `CloudTrail` prefix, and does not check prefixes for other AWS services\. If you want to copy CloudTrail events contained in another prefix, you must choose the prefix when you copy trail events\. 
+  To copy trail events to an organization event data store, you must use the management account for the organization\. You cannot use the delegated administrator account to copy trail events to an organization event data store\. 

## Required permissions for copying trail events<a name="cloudtrail-copy-trail-events-permissions"></a>

Before copying trail events, ensure you have all the required permissions for your IAM role and the event data store's S3 bucket\. You only need to update the IAM role permissions if you choose an existing IAM role to copy trail events\. If you choose to create a new IAM role, CloudTrail provides all necessary permissions for the role\.

If the source S3 bucket uses a KMS key for data encryption, ensure that the KMS key policy allows CloudTrail to decrypt data in the bucket\. If the source S3 bucket uses multiple KMS keys, you must update each key's policy to allow CloudTrail to decrypt the data in the bucket\.

**Topics**
+ [IAM permissions for copying trail events](#cloudtrail-copy-trail-events-permissions-iam)
+ [Amazon S3 bucket policy for copying trail events](#cloudtrail-copy-trail-events-permissions-s3)
+ [KMS key policy for decrypting data in the source S3 bucket](#cloudtrail-copy-trail-events-permissions-kms)

### IAM permissions for copying trail events<a name="cloudtrail-copy-trail-events-permissions-iam"></a>

When copying trail events, you have the option to create a new IAM role, or use an existing IAM role\. When you choose a new IAM role, CloudTrail creates an IAM role with the required permissions and no further action is required on your part\.

If you choose an existing role, ensure the IAM role's policies allow CloudTrail to copy trail events to the destination event data store\. This section provides examples of the required IAM role permission and trust policies\.

The following example provides the permissions policy, which allows CloudTrail to copy trail events to the event data store's S3 bucket\. Replace *myBucketName*, *myAccountID*, *region*, *prefix*, and *eventDataStoreArn* with the appropriate values for your configuration\. The *myAccountID* is the AWS account ID used for CloudTrail Lake, which may not be the same as the AWS account ID for the S3 bucket\.

Replace *key\-region*, *keyAccountID*, and *keyID* with the values for the KMS key used to encrypt the source S3 bucket\. You can omit the `AWSCloudTrailImportKeyAccess` statement if the source S3 bucket does not use a KMS key for encryption\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AWSCloudTrailImportBucketAccess",
      "Effect": "Allow",
      "Action": ["s3:ListBucket", "s3:GetBucketAcl"],
      "Resource": [
        "arn:aws:s3:::myBucketName"
      ],
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "myAccountID",
          "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:eventdataStore/eventDataStoreArn"
         }
       }
    },
    {
      "Sid": "AWSCloudTrailImportObjectAccess",
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": [
        "arn:aws:s3:::myBucketName/prefix",
        "arn:aws:s3:::myBucketName/prefix/*"
      ],
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "myAccountID",
          "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:eventdataStore/eventDataStoreArn"
         }
       }
    },
    {
      "Sid": "AWSCloudTrailImportKeyAccess",
      "Effect": "Allow",
      "Action": ["kms:GenerateDataKey","kms:Decrypt"],
      "Resource": [
        "arn:aws:kms:key-region:keyAccountID:key/keyID"
      ]
    }
  ]
}
```

The following example provides the IAM trust policy, which allows CloudTrail to assume an IAM role to copy trail events to the event data store's S3 bucket\. Replace *myAccountID*, *region*, and *eventDataStoreArn* with the appropriate values for your configuration\. The *myAccountID* is the AWS account ID used for CloudTrail Lake, which may not be the same as the AWS account ID for the S3 bucket\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "sts:AssumeRole",
      "Condition": {
        "StringEquals": {
          "aws:SourceAccount": "myAccountID",
          "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:eventdataStore/eventDataStoreArn"
        }
      }
    }
  ]
}
```

### Amazon S3 bucket policy for copying trail events<a name="cloudtrail-copy-trail-events-permissions-s3"></a>

By default, Amazon S3 buckets and objects are private\. Only the resource owner \(the AWS account that created the bucket\) can access the bucket and objects it contains\. The resource owner can grant access permissions to other resources and users by writing an access policy\.

Before you copy trail events, you must update the S3 bucket policy for the destination event data store to allow CloudTrail to copy trail events to the bucket\.

You can add the following statement to the event data store's S3 bucket policy to grant these permissions\. Replace *roleArn* and *myBucketName* with the appropriate values for your configuration\.

****

```
{
  "Sid": "AWSCloudTrailImportBucketAccess",
  "Effect": "Allow",
  "Action": [
    "s3:ListBucket",
    "s3:GetBucketAcl",
    "s3:GetObject"
  ],
  "Principal": {
    "AWS": "roleArn"
  },
  "Resource": [
    "arn:aws:s3:::myBucketName",
    "arn:aws:s3:::myBucketName/*"
  ]
},
```

### KMS key policy for decrypting data in the source S3 bucket<a name="cloudtrail-copy-trail-events-permissions-kms"></a>

If the source S3 bucket uses a KMS key for data encryption, ensure the KMS key policy provides CloudTrail with the `kms:Decrypt` and `kms:GenerateDataKey` permissions required to copy trail events from an S3 bucket with SSE\-KMS encryption enabled\. If your source S3 bucket uses multiple KMS keys, you must update each key's policy\. Updating the KMS key policy allows CloudTrail to decrypt data in the source S3 bucket, run validation checks to ensure that events conform to CloudTrail standards, and copy events into the CloudTrail Lake event data store\. 

The following example provides the KMS key policy, which allows CloudTrail to decrypt the data in the source S3 bucket\. Replace *roleArn*, *myBucketName*, *myAccountID*, *region*, and *eventDataStoreArn* with the appropriate values for your configuration\. The *myAccountID* is the AWS account ID used for CloudTrail Lake, which may not be the same as the AWS account ID for the S3 bucket\.

```
{
  "Sid": "AWSCloudTrailImportDecrypt",
  "Effect": "Allow",
  "Action": [
          "kms:Decrypt",
          "kms:GenerateDataKey"
  ],
  "Principal": {
    "AWS": "roleArn"
  },
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "kms:EncryptionContext:aws:s3:arn": "arn:aws:s3:::myBucketName/*"
    },
    "StringEquals": {
      "aws:SourceAccount": "myAccountID",
      "aws:SourceArn": "arn:aws:cloudtrail:region:myAccountID:eventdataStore/eventDataStoreArn"
    }
  }
}
```

## Copy trail events to CloudTrail Lake using the CloudTrail console<a name="cloudtrail-copy-trail-events"></a>

Use the following procedure to copy trail events to CloudTrail Lake\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Trails** in the left navigation pane of the CloudTrail console\.

1. On the **Trails** page, choose the trail, and then choose **Copy events to Lake**\. If the source S3 bucket for the trail uses a KMS key for data encryption, ensure that the KMS key policy allows CloudTrail to decrypt data in the bucket\. If the source S3 bucket uses multiple KMS keys, you must update each key's policy to allow CloudTrail to decrypt data in the bucket\. For more information about updating the KMS key policy, see [KMS key policy for decrypting data in the source S3 bucket](#cloudtrail-copy-trail-events-permissions-kms)\. 

1.  \(Optional\) By default, CloudTrail only copies CloudTrail events contained in the S3 bucket's `CloudTrail` prefix and the prefixes inside the `CloudTrail` prefix, and does not check prefixes for other AWS services\. If you want to copy CloudTrail events contained in another prefix, choose **Enter S3 URI**, and then choose **Browse S3** to browse to the prefix\. 

1. \(Optional\) For **Event source**, choose the time range for copying the events\. If you choose a time range, CloudTrail checks the prefix and log file name to verify the name contains a date between the chosen start and end date before attempting to copy trail events\. You can choose a **Relative range** or an **Absolute range**\. To avoid duplicating events between the source trail and destination event data store, choose a time range that is earlier than the creation of the event data store\.
   + If you choose **Relative range**, you can choose to copy events logged in the last 5 minutes, 30 minutes, 1 hour, 6 hours, or a custom range\. CloudTrail copies the events logged within the chosen time period\.
   + If you choose **Absolute range**, you can choose a specific start and end date\. CloudTrail copies the events that occurred between the chosen start and end dates\.

1. For **Delivery location**, choose the destination event data store from the drop\-down list\. The S3 bucket policy for the event data store must grant CloudTrail access to copy trail events into the bucket\. For more information about updating the S3 bucket policy, see [Amazon S3 bucket policy for copying trail events](#cloudtrail-copy-trail-events-permissions-s3)\.

1. For **Permissions**, choose from the following IAM role options\. If you choose an existing IAM role, verify that the IAM role policy provides the necessary permissions\. For more information about updating the IAM role permissions, see [IAM permissions for copying trail events](#cloudtrail-copy-trail-events-permissions-iam)\.
   + Choose **Create a new role \(recommended\)** to create a new IAM role\. For **Enter IAM role name**, enter a name for the role\. CloudTrail automatically creates the necessary permissions for this new role\.
   + Choose **Use a custom IAM role** to use a custom IAM role that is not listed\. For **Enter IAM role ARN**, enter the IAM ARN\.
   + Choose **Use an existing role** to choose an existing IAM role from the drop\-down list\.

1. Choose **Copy events**\.

1. You are prompted to confirm the copy\. When you are ready to confirm, choose **Copy trail events to Lake**, and then choose **Copy events**\.

1. On the **Copy details** page, you can see the copy status and review any failures\. When a trail event copy completes, its **Copy status** is set to either **Completed** if there were no errors, or **Failed** if errors occurred\.
**Note**  
Details shown on the event copy details page are not in real\-time\. The actual values for details such as **Prefixes copied** may be higher than what is shown on the page\. CloudTrail updates the details incrementally over the course of the event copy\.

1. If the **Copy status** is **Failed**, fix any errors shown in **Copy failures**, and then choose **Retry copy**\. When you retry a copy, CloudTrail resumes the copy at the location where the failure occurred\. 

For more information about viewing the details of a trail event copy, see [Event copy details](copy-trail-details.md)\.