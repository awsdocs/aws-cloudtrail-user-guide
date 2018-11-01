# Encrypting CloudTrail Log Files with AWS KMS–Managed Keys \(SSE\-KMS\)<a name="encrypting-cloudtrail-log-files-with-aws-kms"></a>

 By default, the log files delivered by CloudTrail to your bucket are encrypted by Amazon [server\-side encryption with Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\. To provide a security layer that is directly manageable, you can instead use [server\-side encryption with AWS KMS–managed keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) for your CloudTrail log files\.

**Note**  
Enabling server\-side encryption encrypts the log files but not the digest files with SSE\-KMS\. Digest files are encrypted with [Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\.

To use SSE\-KMS with CloudTrail, you create and manage a KMS key, also known as a [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)\. You attach a policy to the key that determines which users can use the key for encrypting and decrypting CloudTrail log files\. The decryption is seamless through S3\. When authorized users of the key read CloudTrail log files, S3 manages the decryption, and the authorized users are able to read log files in unencrypted form\.

 This approach has the following advantages: 
+  You can create and manage the CMK encryption keys yourself\. 
+  You can use a single CMK to encrypt and decrypt log files for multiple accounts across all regions\. 
+  You have control over who can use your key for encrypting and decrypting CloudTrail log files\. You can assign permissions for the key to the users in your organization according to your requirements\. 
+  You have enhanced security\. With this feature, in order to read log files, you now need to meet two conditions: 
  + You must have S3 read permission on the bucket\. 
  + You must be granted decrypt permission by the CMK policy\.
+ Because S3 automatically decrypts the log files for requests from users authorized to use the CMK, SSE\-KMS encryption for CloudTrail log files is backward\-compatible with applications that read CloudTrail log data\. 

**Note**  
The key that you choose must be in the same region as the S3 bucket that receives your log files\. To verify the region for an S3 bucket, inspect its properties in the S3 console\. 

## Enabling Log File Encryption<a name="encrypting-cloudtrail-log-files-with-aws-kms-enabling"></a>

**Note**  
If you create a CMK in the CloudTrail console, CloudTrail adds the required CMK policy sections for you\. Follow these procedures if you created a key in the IAM console or AWS CLI and you need to manually add the required policy sections\.

To enable SSE\-KMS encryption for CloudTrail log files, perform the following high\-level steps:

1.  Create a CMK\. 
   + For information about creating a CMK with the AWS Management Console, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. 
   + For information about creating a CMK with the AWS CLI, see [create\-key](https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html)\.
**Note**  
The CMK that you choose must be in the same region as the S3 bucket that receives your log files\. To verify the region for an S3 bucket, inspect the bucket's properties in the S3 console\. 

1. Add policy sections to the key that enable CloudTrail to encrypt and users to decrypt log files\. 
   + For information about what to include in the policy, see [AWS KMS Key Policy for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\. 
**Warning**  
Be sure to include decrypt permissions in the policy for all users who need to read log files\. If you do not perform this step before adding the key to your trail configuration, users without decrypt permissions cannot read encrypted files\.
   + For information about editing a policy with the IAM console, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.
   + For information about attaching a policy to a CMK with the AWS CLI, see [put\-key\-policy](https://docs.aws.amazon.com/cli/latest/reference/kms/put-key-policy.html)\.

1. Update your trail to use the CMK whose policy you modified for CloudTrail\. 
   + To update your trail configuration by using the CloudTrail console, see [Updating a Trail to Use Your CMK](create-kms-key-policy-for-cloudtrail-update-trail.md)\.
   + To update your trail configuration by using the AWS CLI, see [Enabling and disabling CloudTrail log file encryption with the AWS CLI](cloudtrail-log-file-encryption-cli.md)\. 

The next section describes the policy sections that your CMK policy requires for use with CloudTrail\.