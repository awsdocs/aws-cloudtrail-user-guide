# Enabling and disabling CloudTrail log file encryption with the AWS CLI<a name="cloudtrail-log-file-encryption-cli"></a>

This topic describes how to enable and disable SSE\-KMS log file encryption for CloudTrail by using the AWS CLI\. For background information, see [Encrypting CloudTrail log files with AWS KMS keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)\.

## Enabling CloudTrail log file encryption by using the AWS CLI<a name="cloudtrail-log-file-encryption-cli-enable"></a>
+ [Enable log file encryption for a trail](#log-encryption-trail)
+ [Enable log file encryption for an event data store](#log-encryption-eds)<a name="log-encryption-trail"></a>

**Enable log file encryption for a trail**

1. Create a key with the AWS CLI\. The key that you create must be in the same region as the S3 bucket that receives your CloudTrail log files\. For this step, you use the AWS KMS [https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html](https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html) command\.

1. Get the existing key policy so that you can modify it for use with CloudTrail\. You can retrieve the key policy with the AWS KMS [https://docs.aws.amazon.com/cli/latest/reference/kms/get-key-policy.html](https://docs.aws.amazon.com/cli/latest/reference/kms/get-key-policy.html) command\. 

1. Add required sections to the key policy so that CloudTrail can encrypt and users can decrypt your log files\. Be sure that all users who read the log files are granted decrypt permissions\. Do not change existing sections of the policy\. For information about the policy sections to include, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

1. Attach the modified JSON policy file to the key by using the AWS KMS [https://docs.aws.amazon.com/cli/latest/reference/kms/put-key-policy.html](https://docs.aws.amazon.com/cli/latest/reference/kms/put-key-policy.html) command\.  

1. Run the CloudTrail `create-trail` or `update-trail` command with the `--kms-key-id` parameter\. This command enables log encryption\.

   ```
   aws cloudtrail update-trail --name Default --kms-key-id alias/MyKmsKey
   ```

   The `--kms-key-id` parameter specifies the key whose policy you modified for CloudTrail\. It can be any one of the following formats: 
   + **Alias Name**\. Example: `alias/MyAliasName`
   + **Alias ARN**\. Example: `arn:aws:kms:us-east-2:123456789012:alias/MyAliasName` 
   + **Key ARN**\. Example: `arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012` 
   + **Globally unique key ID\.** Example: `12345678-1234-1234-1234-123456789012` 

   The following is an example response:

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

   The presence of the `KmsKeyId` element indicates that log file encryption has been enabled\. The encrypted log files should appear in your bucket in about 15 minutes\.<a name="log-encryption-eds"></a>

**Enable log file encryption for an event data store**

1. Create a key with the AWS CLI\. The key that you create must be in the same region as the event data store\. For this step, run the AWS KMS [https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html](https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html) command\.

1. Get the existing key policy to edit for use with CloudTrail\. You can get the key policy by running the AWS KMS [https://docs.aws.amazon.com/cli/latest/reference/kms/get-key-policy.html](https://docs.aws.amazon.com/cli/latest/reference/kms/get-key-policy.html) command\. 

1. Add required sections to the key policy so that CloudTrail can encrypt and users can decrypt your log files\. Be sure that all users who read the log files are granted decrypt permissions\. Do not change existing sections of the policy\. For information about the policy sections to include, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

1. Attach the edited JSON policy file to the key by running the AWS KMS [put\-key\-policy](https://docs.aws.amazon.com/cli/latest/reference/kms/put-key-policy.html) command\.

1. Run the CloudTrail `create-event-data-store` or `update-event-data-store` command, and add the `--kms-key-id` parameter\. This command enables log encryption\.

   ```
   aws cloudtrail update-event-data-store --name my-event-data-store --kms-key-id alias/MyKmsKey
   ```

   The `--kms-key-id` parameter specifies the key whose policy you modified for CloudTrail\. It can be any one of the following four formats: 
   + **Alias Name**\. Example: `alias/MyAliasName`
   + **Alias ARN**\. Example: `arn:aws:kms:us-east-2:123456789012:alias/MyAliasName` 
   + **Key ARN**\. Example: `arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012` 
   + **Globally unique key ID\.** Example: `12345678-1234-1234-1234-123456789012` 

   The following is an example response:

   ```
   {
       "Name": "my-event-data-store",
       "ARN": "arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLEf852-4e8f-8bd1-bcf6cEXAMPLE",
       "RetentionPeriod": "90",
       "KmsKeyId": "arn:aws:kms:us-east-1:123456789012:key/12345678-1234-1234-1234-123456789012"
       "MultiRegionEnabled": false,
       "OrganizationEnabled": false,
       "TerminationProtectionEnabled": true,
       "AdvancedEventSelectors": [{
           "Name": "Select all external events",
           "FieldSelectors": [{
               "Field": "eventCategory",
               "Equals": [
                   "ActivityAuditLog"
               ]
           }]
       }]
   }
   ```

   The presence of the `KmsKeyId` element indicates that log file encryption has been enabled\. The encrypted log files should appear in your event data store in about 15 minutes\.

## Disabling CloudTrail log file encryption by using the AWS CLI<a name="cloudtrail-log-file-encryption-cli-disable"></a>

To stop encrypting logs on a trail, run `update-trail` and pass an empty string to the `kms-key-id` parameter: 

```
aws cloudtrail update-trail --name my-test-trail --kms-key-id ""
```

The following is an example response:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "Default", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/Default", 
    "LogFileValidationEnabled": false, 
    "S3BucketName": "my-bucket-name"
}
```

**Important**  
You cannot stop log file encryption on an event data store\.