# Updating a Trail to Use Your CMK<a name="create-kms-key-policy-for-cloudtrail-update-trail"></a>

To update a trail to use the customer master key \(CMK\) that you modified for CloudTrail, complete the following steps in the CloudTrail console\.

**Note**  
Updating a trail with the following procedure encrypts the log files but not the digest files with SSE\-KMS\.  Digest files are encrypted with [Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\.

To update a trail using the AWS CLI, see [Enabling and disabling CloudTrail log file encryption with the AWS CLI](cloudtrail-log-file-encryption-cli.md)\.

**To update a trail to use your CMK**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Trails** and then choose a trail\.

1. For **Storage location**, choose the pencil icon\.

1. Choose **Advanced**\. 

1. For **Encrypt log files**, choose **Yes** to have CloudTrail encrypt your log files with the CMK\. 

1. For **Create a new KMS key**, choose **No**\.

1. For **KMS key**, choose the CMK alias whose policy you modified for use with CloudTrail\.
**Note**  
Choose a CMK that is in the same region as the S3 bucket that receives your log files\. To verify the region for an S3 bucket, inspect its properties in the S3 console\.

   You can type the alias name, ARN, or the globally unique key ID\. If the CMK belongs to another account, verify that the key policy has permissions that enable you to use it\. The value can be one of the following formats:
   + **Alias Name**: `alias/MyAliasName`
   + **Alias ARN**: `arn:aws:kms:region:123456789012:alias/MyAliasName` 
   + **Key ARN**: `arn:aws:kms:region:123456789012:key/12345678-1234-1234-1234-123456789012` 
   + **Globally unique key ID**: `12345678-1234-1234-1234-123456789012` 

1. Choose **Save**\.
**Note**  
If the CMK that you chose is disabled or is pending deletion, you cannot save the trail with that CMK\. You can enable the CMK or choose another one\. For more information, see [How Key State Affects Use of a Customer Master Key](https://docs.aws.amazon.com/kms/latest/developerguide/key-state.html)\.