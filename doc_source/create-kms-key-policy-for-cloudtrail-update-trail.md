# Updating a resource to use your KMS key<a name="create-kms-key-policy-for-cloudtrail-update-trail"></a>

In the AWS CloudTrail console, update a trail or an event data store to use an AWS Key Management Service key\. Be aware that using your own KMS key incurs AWS KMS costs for encryption and decryption\. For more information, see [AWS Key Management Service Pricing](http://aws.amazon.com/kms/pricing/)\.

**Topics**
+ [Update a trail to use a KMS key](#kms-key-policy-update-trail)
+ [Update an event data store to use a KMS key](#kms-key-policy-update-eds)

## Update a trail to use a KMS key<a name="kms-key-policy-update-trail"></a>

To update a trail to use the AWS KMS key that you modified for CloudTrail, complete the following steps in the CloudTrail console\.

**Note**  
Updating a trail with the following procedure encrypts the log files but not the digest files with SSE\-KMS\. Digest files are encrypted with [Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\.  
If you are using an existing S3 bucket with an [S3 Bucket Key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html), CloudTrail must be allowed permission in the key policy to use the AWS KMS actions `GenerateDataKey` and `DescribeKey`\. If `cloudtrail.amazonaws.com` is not granted those permissions in the key policy, you cannot create or update a trail\.

To update a trail using the AWS CLI, see [Enabling and disabling CloudTrail log file encryption with the AWS CLI](cloudtrail-log-file-encryption-cli.md)\.

**To update a trail to use your KMS key**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Trails** and then choose a trail name\.

1. In **General details**, choose **Edit**\.

1. For **Log file SSE\-KMS encryption**, choose **Enabled** if you want to encrypt your log files using SSE\-KMS encryption instead of SSE\-S3 encryption\. The default is **Enabled**\. If you don't enable SSE\-KMS encryption, your logs are encrypted using SSE\-S3 encryption\. For more information about SSE\-KMS encryption, see [Using server\-side encryption with AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)\. For more information about SSE\-S3 encryption, see [Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html)\.

   Choose **Existing** to update your trail with your AWS KMS key\. Choose a KMS key that is in the same region as the S3 bucket that receives your log files\. To verify the region for an S3 bucket, view its properties in the S3 console\.
**Note**  
You can also type the ARN of a key from another account\. For more information, see [Updating a resource to use your KMS key](#create-kms-key-policy-for-cloudtrail-update-trail)\. The key policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\. For information about manually editing the key policy, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

   In **AWS KMS Alias**, specify the alias for which you changed the policy for use with CloudTrail, in the format `alias/`*MyAliasName*\. For more information, see [Updating a resource to use your KMS key](#create-kms-key-policy-for-cloudtrail-update-trail)\.

   You can type the alias name, ARN, or the globally unique key ID\. If the KMS key belongs to another account, verify that the key policy has permissions that enable you to use it\. The value can be one of the following formats:
   + **Alias Name**: `alias/MyAliasName`
   + **Alias ARN**: `arn:aws:kms:region:123456789012:alias/MyAliasName` 
   + **Key ARN**: `arn:aws:kms:region:123456789012:key/12345678-1234-1234-1234-123456789012` 
   + **Globally unique key ID**: `12345678-1234-1234-1234-123456789012` 

1. Choose **Update trail**\.
**Note**  
If the KMS key that you chose is disabled or is pending deletion, you cannot save the trail with that KMS key\. You can enable the KMS key or choose another one\. For more information, see [Key state: Effect on your KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/key-state.html) in the *AWS Key Management Service Developer Guide*\.

## Update an event data store to use a KMS key<a name="kms-key-policy-update-eds"></a>

To update an event data store to use the AWS KMS key that you modified for CloudTrail, complete the following steps in the CloudTrail console\.

To update an event data store by using the AWS CLI, see [Update an event data store](query-lake-cli.md#lake-cli-update-eds)\.

**Important**  
Disabling or deleting the KMS key, or removing CloudTrail permissions on the key, prevents CloudTrail from ingesting events into the event data store, and prevents users from querying data in the event data store that was encrypted with the key\. After you associate an event data store with a KMS key, the KMS key cannot be removed or changed\. Before you disable or delete a KMS key that you are using with an event data store, delete or back up your event data store\.

**To update an event data store to use your KMS key**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. In the navigation pane, choose **Event data stores** in **Lake**\. Choose an event data store to update\.

1. In **General details**, choose **Edit**\.

1. For **Encryption**, if it is not already enabled, choose **Use my own AWS KMS key** to encrypt your log files with your own KMS key\.

   Choose **Existing** to update your event data store with your KMS key\. Choose a KMS key that is in the same region as the event data store\. A key from another account is not supported\.

   In **Enter AWS KMS Alias**, specify the alias for which you changed the policy for use with CloudTrail, in the format `alias/`*MyAliasName*\. For more information, see [Updating a resource to use your KMS key](#create-kms-key-policy-for-cloudtrail-update-trail)\.

   You can choose an alias, or use the globally unique key ID\. The value can be one of the following formats:
   + **Alias Name**: `alias/MyAliasName`
   + **Alias ARN**: `arn:aws:kms:region:123456789012:alias/MyAliasName` 
   + **Key ARN**: `arn:aws:kms:region:123456789012:key/12345678-1234-1234-1234-123456789012` 
   + **Globally unique key ID**: `12345678-1234-1234-1234-123456789012` 

1. Choose **Save changes**\.
**Note**  
If the KMS key that you chose is disabled or is pending deletion, you cannot save the event data store configuration with that KMS key\. You can enable the KMS key, or choose a different key\. For more information, see [Key state: Effect on your KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/key-state.html) in the *AWS Key Management Service Developer Guide*\.