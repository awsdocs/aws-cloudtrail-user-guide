# Configure AWS KMS Key Policies for CloudTrail<a name="create-kms-key-policy-for-cloudtrail"></a>

You can create a customer master key \(CMK\) in three ways:
+ The CloudTrail console
+ The IAM console
+ The AWS CLI

**Note**  
If you create a CMK in the CloudTrail console, CloudTrail adds the required CMK policy for you\. You do not need to manually add the policy statements\. See [Default Key Policy Created in CloudTrail Console](default-cmk-policy.md)\.

If you create a CMK in the IAM console or the AWS CLI, you must add policy sections to the key so that you can use it with CloudTrail\. The policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\.

See the following resources:
+ To create a CMK with the AWS CLI, see [create\-key](https://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html)\. 
+ To edit a CMK policy for CloudTrail, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.
+ For technical details on how CloudTrail uses AWS KMS, see [How AWS CloudTrail Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-cloudtrail.html) in the *AWS Key Management Service Developer Guide*\.

## Required CMK policy sections for use with CloudTrail<a name="create-kms-key-policy-for-cloudtrail-policy-sections"></a>

If you created a CMK with the IAM console or the AWS CLI, then you must, at minimum, add three statements to your CMK policy for it to work with CloudTrail\. You do not need to manually add statements for a CMK that is created using the CloudTrail console\.

1. Enable CloudTrail log encrypt permissions\. See [Granting encrypt permissions](#create-kms-key-policy-for-cloudtrail-encrypt)\.

1. Enable CloudTrail log decrypt permissions\. See [Granting decrypt permissions](#create-kms-key-policy-for-cloudtrail-decrypt)\.

1. Enable CloudTrail to describe CMK properties\. See [Enable CloudTrail to describe CMK properties](#create-kms-key-policy-for-cloudtrail-describe)\.

**Note**  
When you add the new sections to your CMK policy, do not change any existing sections in the policy\. 

**Warning**  
If encryption is enabled on a trail and the CMK is disabled or the CMK policy is not correctly configured for CloudTrail, CloudTrail will not deliver logs until the CMK issue is corrected\.

## Granting encrypt permissions<a name="create-kms-key-policy-for-cloudtrail-encrypt"></a>

**Example Allow CloudTrail to encrypt logs on behalf of specific accounts**  
CloudTrail needs explicit permission to use the CMK to encrypt logs on behalf of specific accounts\. To specify an account, add the following required statement to your CMK policy, modifying *aws\-account\-id* as necessary\. You can add additional account IDs to the `EncryptionContext` section to enable those accounts to use CloudTrail to use your CMK to encrypt log files\.

```
{
  "Sid": "Allow CloudTrail to encrypt logs",
  "Effect": "Allow",
  "Principal": {
    "Service": "cloudtrail.amazonaws.com"
  },
  "Action": "kms:GenerateDataKey*",
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "kms:EncryptionContext:aws:cloudtrail:arn": [
        "arn:aws:cloudtrail:*:aws-account-id:trail/*"
      ]
    }
  }
}
```

**Example**  
The following example policy statement illustrates how another account can use your CMK to encrypt CloudTrail logs\.

**Scenario**
+ Your CMK is in account 111111111111\.
+ Both you and account 222222222222 will encrypt logs\.

In the policy, you add one or more accounts that will encrypt with your key to the CloudTrail **EncryptionContext**\. This restricts CloudTrail to using your key to encrypt logs only for those accounts that you specify\. Giving the root of account 222222222222 permission to encrypt logs delegates the administrator of that account to allocate encrypt permissions as required to other users in account 222222222222 by changing their IAM user policies\.

CMK policy statement:

```
{
  "Sid": "Enable CloudTrail Encrypt Permissions",
  "Effect": "Allow",
  "Principal": {
    "Service": "cloudtrail.amazonaws.com"
  },
  "Action": "kms:GenerateDataKey*",
  "Resource": "*",
  "Condition": {
    "StringLike": {
      "kms:EncryptionContext:aws:cloudtrail:arn": [
        "arn:aws:cloudtrail:*:111111111111:trail/*",
        "arn:aws:cloudtrail:*:222222222222:trail/*"
      ]
    }
  }
}
```

For steps on editing a CMK policy for use with CloudTrail, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the AWS Key Management Service Developer Guide\.

## Granting decrypt permissions<a name="create-kms-key-policy-for-cloudtrail-decrypt"></a>

Before you add your CMK to your CloudTrail configuration, it is important to give decrypt permissions to all users who require them\. Users who have encrypt permissions but no decrypt permissions will not be able to read encrypted logs\.

**Enable CloudTrail log decrypt permissions**  
Users of your key must be given explicit permissions to read the log files that CloudTrail has encrypted\. To enable users to read encrypted logs, add the following required statement to your CMK policy, modifying the `Principal` section to add a line for every principal \(role or user\) that you want to be able decrypt by using your CMK\.

```
{
  "Sid": "Enable CloudTrail log decrypt permissions",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::aws-account-id:user/username"
  },
  "Action": "kms:Decrypt",
  "Resource": "*",
  "Condition": {
    "Null": {
      "kms:EncryptionContext:aws:cloudtrail:arn": "false"
    }
  }
}
```

### Allow users in your account to decrypt with your CMK<a name="create-kms-key-policy-for-cloudtrail-decrypt-your-account"></a>

**Example**  
This policy statement illustrates how to allow an IAM user or role in your account to use your key to read the encrypted logs in your account's S3 bucket\.

**Example Scenario**  
+ Your CMK, S3 bucket, and IAM user Bob are in account 111111111111\.
+ You give IAM user Bob permission to decrypt CloudTrail logs in the S3 bucket\.

In the key policy, you enable CloudTrail log decrypt permissions for IAM user Bob\.

CMK policy statement:

```
{
  "Sid": "Enable CloudTrail log decrypt permissions",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::111111111111:user/Bob"
  },
  "Action": "kms:Decrypt",
  "Resource": "*",
  "Condition": {
    "Null": {
      "kms:EncryptionContext:aws:cloudtrail:arn": "false"
    }
  }
}
```

**Topics**

### Allow users in other accounts to decrypt with your CMK<a name="create-kms-key-policy-for-cloudtrail-decrypt-other-accounts"></a>

You can allow users in other accounts to use your CMK to decrypt logs\. The changes required to your key policy depend on whether the S3 bucket is in your account or in another account\.

#### Allow users of a bucket in a different account to decrypt logs<a name="create-kms-key-policy-for-cloudtrail-decrypt-different-bucket"></a>

**Example**  
This policy statement illustrates how to allow an IAM user or role in another account to use your key to read encrypted logs from an S3 bucket in the other account\.

**Scenario**
+ Your CMK is in account 111111111111\.
+ The IAM user Alice and S3 bucket are in account 222222222222\.

In this case, you give CloudTrail permission to decrypt logs under account 222222222222, and you give Alice's IAM user policy permission to use your key `KeyA`, which is in account 111111111111\. 

CMK policy statement:

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
  "Resource": "*",
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
      "Resource": "arn:aws:kms:us-west-2:111111111111:key/keyA"
    }
  ]
}
```

#### Allow users in a different account to decrypt logs from your bucket<a name="create-kms-key-policy-for-cloudtrail-decrypt-same-bucket"></a>

**Example**  
This policy illustrates how another account can use your key to read encrypted logs from your S3 bucket\.

**Example Scenario**  
+ Your CMK and S3 bucket are in account 111111111111\.
+ The user who will read logs from your bucket is in account 222222222222\.

To enable this scenario, you enable decrypt permissions for the IAM role **CloudTrailReadRole** in your account, and then give the other account permission to assume that role\.

CMK policy statement:

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
  "Resource": "*",
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

For steps on editing a CMK policy for use with CloudTrail, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.

## Enable CloudTrail to describe CMK properties<a name="create-kms-key-policy-for-cloudtrail-describe"></a>

CloudTrail requires the ability to describe the properties of the CMK\. To enable this functionality, add the following required statement as is to your CMK policy\. This statement does not grant CloudTrail any permissions beyond the other permissions that you specify\.

```
{
  "Sid": "Allow CloudTrail access",
  "Effect": "Allow",
  "Principal": {
    "Service": "cloudtrail.amazonaws.com"
  },
  "Action": "kms:DescribeKey",
  "Resource": "*"
}
```

For more information about editing CMK policies, see [Editing a Key Policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.