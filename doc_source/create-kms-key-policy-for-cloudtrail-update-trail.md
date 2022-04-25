# Updating a trail to use your KMS key<a name="create-kms-key-policy-for-cloudtrail-update-trail"></a>

To update a trail to use the AWS KMS key that you modified for CloudTrail, complete the following steps in the CloudTrail console\.

**Note**  
Updating a trail with the following procedure encrypts the log files but not the digest files with SSE\-KMS\. Digest files are encrypted with [Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\.  
If you are using an existing S3 bucket with an [S3 Bucket Key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html), CloudTrail must be allowed permission in the key policy to use the AWS KMS actions `GenerateDataKey` and `DescribeKey`\. If `cloudtrail.amazonaws.com` is not granted those permissions in the key policy, you cannot create or update a trail\.

To update a trail using the AWS CLI, see [Enabling and disabling CloudTrail log file encryption with the AWS CLI](cloudtrail-log-file-encryption-cli.md)\.

**To update a trail to use your KMS key**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Trails** and then choose a trail name\.

1. In **General details**, choose **Edit**\.

1. For **Log file SSE\-KMS encryption**, if it is not already enabled, choose **Enabled** to encrypt your log files with SSE\-KMS instead of SSE\-S3\. The default is **Enabled**\. For more information about this encryption type, see [Protecting Data Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\.

   Choose **Existing** to update your trail with your AWS KMS key\. Choose a KMS key that is in the same region as the S3 bucket that receives your log files\. To verify the region for an S3 bucket, view its properties in the S3 console\.
**Note**  
You can also type the ARN of a key from another account\. For more information, see [Updating a trail to use your KMS key](#create-kms-key-policy-for-cloudtrail-update-trail)\. The key policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\. For information about manually editing the key policy, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

   In **AWS KMS Alias**, specify the alias for which you chaned the policy for use with CloudTrail, in the format `alias/`*MyAliasName*\. For more information, see [Updating a trail to use your KMS key](#create-kms-key-policy-for-cloudtrail-update-trail)\.

   You can type the alias name, ARN, or the globally unique key ID\. If the KMS key belongs to another account, verify that the key policy has permissions that enable you to use it\. The value can be one of the following formats:
   + **Alias Name**: `alias/MyAliasName`
   + **Alias ARN**: `arn:aws:kms:region:123456789012:alias/MyAliasName` 
   + **Key ARN**: `arn:aws:kms:region:123456789012:key/12345678-1234-1234-1234-123456789012` 
   + **Globally unique key ID**: `12345678-1234-1234-1234-123456789012` 

1. Choose **Update trail**\.
**Note**  
If the KMS key that you chose is disabled or is pending deletion, you cannot save the trail with that KMS key\. You can enable the KMS key or choose another one\. For more information, see [Key state: Effect on your KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/key-state.html) in the *AWS Key Management Service Developer Guide*\.