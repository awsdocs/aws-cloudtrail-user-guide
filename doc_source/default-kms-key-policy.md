# Default KMS key policy created in CloudTrail console<a name="default-kms-key-policy"></a>

If you create an AWS KMS key in the CloudTrail console, the following policy is automatically created for you\. The policy allows these permissions:
+ Allows AWS account \(root\) permissions for the KMS key\.
+ Allows CloudTrail to encrypt log files under the KMS key and describe the KMS key\.
+ Allows all users in the specified accounts to decrypt log files\.
+ Allows all users in the specified account to create a KMS alias for the KMS key\.
+ Enables cross\-account log decryption for the account ID of the account that created the trail\. 

```
{
    "Version": "2012-10-17",
    "Id": "Key policy created by CloudTrail",
    "Statement": [
        {
            "Sid": "Enable IAM User Permissions",
            "Effect": "Allow",
            "Principal": {"AWS": [
                "arn:aws:iam::account-id:root",
                "arn:aws:iam::account-id:user/username"
            ]},
            "Action": "kms:*",
            "Resource": "arn:aws:s3:::myBucketName"
        },
        {
            "Sid": "Allow CloudTrail to encrypt logs",
            "Effect": "Allow",
            "Principal": {"Service": ["cloudtrail.amazonaws.com"]},
            "Action": "kms:GenerateDataKey*",
            "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
            "Condition": {"StringLike": {"kms:EncryptionContext:aws:cloudtrail:arn": "arn:aws:cloudtrail:*:account-id:trail/*"}}
        },
        {
            "Sid": "Allow CloudTrail to describe key",
            "Effect": "Allow",
            "Principal": {"Service": ["cloudtrail.amazonaws.com"]},
            "Action": "kms:DescribeKey",
            "Resource": "arn:aws:kms:Region:account_ID:key/key_ID"
        },
        {
            "Sid": "Allow principals in the account to decrypt log files",
            "Effect": "Allow",
            "Principal": {"AWS": "*"},
            "Action": [
                "kms:Decrypt",
                "kms:ReEncryptFrom"
            ],
            "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
            "Condition": {
                "StringEquals": {"kms:CallerAccount": "account-id"},
                "StringLike": {"kms:EncryptionContext:aws:cloudtrail:arn": "arn:aws:cloudtrail:*:account-id:trail/*"}
            }
        },
        {
            "Sid": "Allow alias creation during setup",
            "Effect": "Allow",
            "Principal": {"AWS": "*"},
            "Action": "kms:CreateAlias",
            "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
            "Condition": {"StringEquals": {
                "kms:ViaService": "ec2.region.amazonaws.com",
                "kms:CallerAccount": "account-id"
            }}
        },
        {
            "Sid": "Enable cross account log decryption",
            "Effect": "Allow",
            "Principal": {"AWS": "*"},
            "Action": [
                "kms:Decrypt",
                "kms:ReEncryptFrom"
            ],
            "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
            "Condition": {
                "StringEquals": {"kms:CallerAccount": "account-id"},
                "StringLike": {"kms:EncryptionContext:aws:cloudtrail:arn": "arn:aws:cloudtrail:*:account-id:trail/*"}
            }
        }
    ]
}
```

**Note**  
The policy's final statement allows cross accounts to decrypt log files with the KMS key\.