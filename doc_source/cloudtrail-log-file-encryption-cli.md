# Enabling and disabling CloudTrail log file encryption with the AWS CLI<a name="cloudtrail-log-file-encryption-cli"></a>

This topic describes how to enable and disable SSE\-KMS log file encryption for CloudTrail by using the AWS CLI\. For background information, see [Encrypting CloudTrail Log Files with AWS KMS–Managed Keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)\.

## Enabling CloudTrail log file encryption by using the AWS CLI<a name="cloudtrail-log-file-encryption-cli-enable"></a>

1. Create a key with the AWS CLI\. The key that you create must be in the same region as the S3 bucket that receives your CloudTrail log files\. For this step, you use the KMS [create\-key](http://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html) command\.

1. Get the existing key policy so that you can modify it for use with CloudTrail\. You can retrieve the key policy with the KMS [get\-key\-policy](http://docs.aws.amazon.com/cli/latest/reference/kms/get-key-policy.html) command\. 

1. Add the necessary sections to the key policy so that CloudTrail can encrypt and users can decrypt your log files\. Make sure that all users who will read the log files are granted decrypt permissions\. Do not modify any existing sections of the policy\. For information on the policy sections to include, see [AWS KMS Key Policy for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

1. Attach the modified \.json policy file to the key by using the KMS [put\-key\-policy](http://docs.aws.amazon.com/cli/latest/reference/kms/put-key-policy.html) command\.  

1. Run the CloudTrail `create-trail` or `update-trail` command with the `--kms-key-id` parameter\. This command will enable log encryption\.

   ```
   aws cloudtrail update-trail --name Default --kms-key-id alias/MyKmsKey
   ```

   The `--kms-key-id` parameter specifies the key whose policy you modified for CloudTrail\. It can be any one of the following four formats: 

   + **Alias Name**\. Example: `alias/MyAliasName`

   + **Alias ARN**\. Example: `arn:aws:kms:us-east-2:123456789012:alias/MyAliasName` 

   + **Key ARN**\. Example: `arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012` 

   + **Globally unique key ID\.** Example: `12345678-1234-1234-1234-123456789012` 

   The response will look like the following:

   ```
   {
       "IncludeGlobalServiceEvents": true, 
       "Name": "Default", 
       "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/Default", 
       "LogFileValidationEnabled": false,
       "KmsKeyId": "arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012", 
       "S3BucketName": "my-bucket-name"
   }
   ```

   The presence of the `KmsKeyId` element indicates that log file encryption has been enabled\. The encrypted log files should appear in your bucket in about 10 minutes\.

## Disabling CloudTrail log file encryption by using the AWS CLI<a name="cloudtrail-log-file-encryption-cli-disable"></a>

 To stop encrypting logs, call `update-trail` and pass an empty string to the `kms-key-id` parameter: 

```
aws cloudtrail update-trail --name Default --kms-key-id ""
```

The response will look like the following:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "Default", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/Default", 
    "LogFileValidationEnabled": false, 
    "S3BucketName": "my-bucket-name"
}
```

The absence of the `KmsKeyId` element indicates that log file encryption is no longer enabled\.