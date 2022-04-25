# Configure AWS KMS key policies for CloudTrail<a name="create-kms-key-policy-for-cloudtrail"></a>

You can create an AWS KMS key in three ways:
+ The CloudTrail console
+ The IAM console
+ The AWS CLI

**Note**  
If you create a KMS key in the CloudTrail console, CloudTrail adds the required KMS key policy for you\. You do not need to manually add the policy statements\. See [Default KMS key policy created in CloudTrail console](default-kms-key-policy.md)\.

If you create a KMS key in the IAM console or the AWS CLI, you must add policy sections to the key so that you can use it with CloudTrail\. The policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\.

See the following resources:
+ To create a KMS key with the AWS CLI, see [create\-key](https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html)\. 
+ To edit a KMS key policy for CloudTrail, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.
+ For technical details on how CloudTrail uses AWS KMS, see [How AWS CloudTrail Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-cloudtrail.html) in the *AWS Key Management Service Developer Guide*\.

## Required KMS key policy sections for use with CloudTrail<a name="create-kms-key-policy-for-cloudtrail-policy-sections"></a>

If you created a KMS key with the IAM console or the AWS CLI, then you must, at minimum, add the following statements to your KMS key policy for it to work with CloudTrail\.

1. Enable CloudTrail log encrypt permissions\. See [Granting encrypt permissions](#create-kms-key-policy-for-cloudtrail-encrypt)\.

1. Enable CloudTrail log decrypt permissions\. See [Granting decrypt permissions](#create-kms-key-policy-for-cloudtrail-decrypt)\. If you are using an existing S3 bucket with an [S3 Bucket Key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html), `kms:Decrypt` permissions are required to create or update a trail with SSE\-KMS encryption enabled\.

1. Enable CloudTrail to describe KMS key properties\. See [Enable CloudTrail to describe KMS key properties](#create-kms-key-policy-for-cloudtrail-describe)\.

As a security best practice, add an `aws:SourceArn` condition key to the KMS key policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail uses the KMS key only for a specific trail or trails\. The value of `aws:SourceArn` is always the trail ARN \(or array of trail ARNs\) that is using the KMS key\. Be sure to add the `aws:SourceArn` condition key to KMS key policies for existing trails\.

The `aws:SourceAccount` condition key is also supported, but not recommended\. The value of `aws:SourceAccount` is the account ID of the trail owner, or for organization trails, the management account ID\.

**Important**  
When you add the new sections to your KMS key policy, do not change any existing sections in the policy\.  
If encryption is enabled on a trail, and the KMS key is disabled, or the KMS key policy is not correctly configured for CloudTrail, CloudTrail cannot deliver logs\.

## Granting encrypt permissions<a name="create-kms-key-policy-for-cloudtrail-encrypt"></a>

**Example Allow CloudTrail to encrypt logs on behalf of specific accounts**  
CloudTrail needs explicit permission to use the KMS key to encrypt logs on behalf of specific accounts\. To specify an account, add the following required statement to your KMS key policy and replace *myBucketName*, *account\-id*, *region*, and *trailName* with the appropriate values for your configuration\. You can add additional account IDs to the `EncryptionContext` section to enable those accounts to use CloudTrail to use your KMS key to encrypt log files\.  
As a security best practice, add an `aws:SourceArn` condition key to the KMS key policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail uses the KMS key only for a specific trail or trails\.

```
{
  "Sid": "Allow CloudTrail to encrypt logs",
  "Effect": "Allow",
  "Principal": {
    "Service": "cloudtrail.amazonaws.com"
  },
  "Action": "kms:GenerateDataKey*",
  "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
  "Condition": {
    "StringLike": {
      "kms:EncryptionContext:aws:cloudtrail:arn": [
        "arn:aws:cloudtrail:*:account-id:trail/*"
      ]
    },
    "StringEquals": {
        "aws:SourceArn": "arn:aws:cloudtrail:region:account-id:trail/trailName"
    }
  }
}
```

**Example**  
The following example policy statement illustrates how another account can use your KMS key to encrypt CloudTrail logs\.

**Scenario**
+ Your KMS key is in account *111111111111*\.
+ Both you and account *222222222222* will encrypt logs\.

In the policy, you add one or more accounts that will encrypt with your key to the CloudTrail **EncryptionContext**\. This restricts CloudTrail to using your key to encrypt logs only for those accounts that you specify\. Giving the root of account *222222222222* permission to encrypt logs delegates the administrator of that account to allocate encrypt permissions as required to other users in account *222222222222* by changing their IAM user policies\. 

As a security best practice, add an `aws:SourceArn` condition key to the KMS key policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail uses the KMS key only for a specific trail or trails\.

KMS key policy statement:

```
{
  "Sid": "Enable CloudTrail Encrypt Permissions",
  "Effect": "Allow",
  "Principal": {
    "Service": "cloudtrail.amazonaws.com"
  },
  "Action": "kms:GenerateDataKey*",
  "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
  "Condition": {
    "StringLike": {
      "kms:EncryptionContext:aws:cloudtrail:arn": [
        "arn:aws:cloudtrail:*:111111111111:trail/*",
        "arn:aws:cloudtrail:*:222222222222:trail/*"
      ]
    },
    "StringEquals": {
        "aws:SourceArn": "arn:aws:cloudtrail:region:account-id:trail/trailName"
    }
  }
}
```

For more information about editing a KMS key policy for use with CloudTrail, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the AWS Key Management Service Developer Guide\.

## Granting decrypt permissions<a name="create-kms-key-policy-for-cloudtrail-decrypt"></a>

Before you add your KMS key to your CloudTrail configuration, it is important to give decrypt permissions to all users who require them\. Users who have encrypt permissions but no decrypt permissions will not be able to read encrypted logs\. If you are using an existing S3 bucket with an [S3 Bucket Key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html), `kms:Decrypt` permissions are required to create or update a trail with SSE\-KMS encryption enabled\.

**Enable CloudTrail log decrypt permissions**  
Users of your key must be given explicit permissions to read the log files that CloudTrail has encrypted\. To enable users to read encrypted logs, add the following required statement to your KMS key policy, modifying the `Principal` section to add a line for every principal \(role or user\) that you want to be able decrypt by using your KMS key\.

```
{
  "Sid": "Enable CloudTrail log decrypt permissions",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::account-id:user/username"
  },
  "Action": "kms:Decrypt",
  "Resource": "arn:aws:s3:::myBucketName",
  "Condition": {
    "Null": {
      "kms:EncryptionContext:aws:cloudtrail:arn": "false"
    }
  }
}
```

### Allow users in your account to decrypt with your KMS key<a name="create-kms-key-policy-for-cloudtrail-decrypt-your-account"></a>

**Example**  
This policy statement illustrates how to allow an IAM user or role in your account to use your key to read the encrypted logs in your account's S3 bucket\.

**Example Scenario**  
+ Your KMS key, S3 bucket, and IAM user Bob are in account *111111111111*\.
+ You give IAM user Bob permission to decrypt CloudTrail logs in the S3 bucket\.

In the key policy, you enable CloudTrail log decrypt permissions for IAM user Bob\.

KMS key policy statement:

```
{
  "Sid": "Enable CloudTrail log decrypt permissions",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::111111111111:user/Bob"
  },
  "Action": "kms:Decrypt",
  "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
  "Condition": {
    "Null": {
      "kms:EncryptionContext:aws:cloudtrail:arn": "false"
    }
  }
}
```

**Topics**

### Allow users in other accounts to decrypt with your KMS key<a name="create-kms-key-policy-for-cloudtrail-decrypt-other-accounts"></a>

You can allow users in other accounts to use your KMS key to decrypt logs\. The changes required to your key policy depend on whether the S3 bucket is in your account or in another account\.

#### Allow users of a bucket in a different account to decrypt logs<a name="create-kms-key-policy-for-cloudtrail-decrypt-different-bucket"></a>

**Example**  
This policy statement illustrates how to allow an IAM user or role in another account to use your key to read encrypted logs from an S3 bucket in the other account\.

**Scenario**
+ Your KMS key is in account *111111111111*\.
+ The IAM user Alice and S3 bucket are in account *222222222222*\.

In this case, you give CloudTrail permission to decrypt logs under account *222222222222*, and you give Alice's IAM user policy permission to use your key *KeyA*, which is in account *111111111111*\. 

KMS key policy statement:

```
{
  "Sid": "Enable encrypted CloudTrail log read access",
  "Effect": "Allow",
  "Principal": {
    "AWS": [
      "arn:aws:iam::222222222222:root"
    ]
  },
  "Action": "kms:Decrypt",
  "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
  "Condition": {
    "Null": {
      "kms:EncryptionContext:aws:cloudtrail:arn": "false"
    }
  }
}
```

Alice's IAM user policy statement:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "kms:Decrypt",
      "Resource": "arn:aws:kms:us-west-2:111111111111:key/KeyA"
    }
  ]
}
```

#### Allow users in a different account to decrypt logs from your bucket<a name="create-kms-key-policy-for-cloudtrail-decrypt-same-bucket"></a>

**Example**  
This policy illustrates how another account can use your key to read encrypted logs from your S3 bucket\.

**Example Scenario**  
+ Your KMS key and S3 bucket are in account *111111111111*\.
+ The user who will read logs from your bucket is in account *222222222222*\.

To enable this scenario, you enable decrypt permissions for the IAM role **CloudTrailReadRole** in your account, and then give the other account permission to assume that role\.

KMS key policy statement:

```
{
  "Sid": "Enable encrypted CloudTrail log read access",
  "Effect": "Allow",
  "Principal": {
    "AWS": [
      "arn:aws:iam::11111111111:role/CloudTrailReadRole"
    ]
  },
  "Action": "kms:Decrypt",
  "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
  "Condition": {
    "Null": {
      "kms:EncryptionContext:aws:cloudtrail:arn": "false"
    }
  }
}
```

**CloudTrailReadRole** trust entity policy statement:

```
{
 "Version": "2012-10-17",
 "Statement": [
   {
     "Sid": "",
     "Effect": "Allow",
     "Principal": {
       "AWS": "arn:aws:iam::222222222222:root"
     },
     "Action": "sts:AssumeRole"
    }
  ]
 }
```

For steps on editing a KMS key policy for use with CloudTrail, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.

## Enable CloudTrail to describe KMS key properties<a name="create-kms-key-policy-for-cloudtrail-describe"></a>

CloudTrail requires the ability to describe the properties of the KMS key\. To enable this functionality, add the following required statement as is to your KMS key policy\. This statement does not grant CloudTrail any permissions beyond the other permissions that you specify\. 

As a security best practice, add an `aws:SourceArn` condition key to the KMS key policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail uses the KMS key only for a specific trail or trails\.

```
{
  "Sid": "Allow CloudTrail access",
  "Effect": "Allow",
  "Principal": {
    "Service": "cloudtrail.amazonaws.com"
  },
  "Action": "kms:DescribeKey",
  "Resource": "arn:aws:kms:Region:account_ID:key/key_ID",
  "Condition": {
    "StringEquals": {
        "aws:SourceArn": "arn:aws:cloudtrail:region:account-id:trail/trailName"
    }
  }
}
```

For more information about editing KMS key policies, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.