# Granting decrypt permissions<a name="create-kms-key-policy-for-cloudtrail-decrypt"></a>

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

## Allow users in your account to decrypt with your CMK<a name="create-kms-key-policy-for-cloudtrail-decrypt-your-account"></a>

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



## Allow users in other accounts to decrypt with your CMK<a name="create-kms-key-policy-for-cloudtrail-decrypt-other-accounts"></a>

You can allow users in other accounts to use your CMK to decrypt logs\. The changes required to your key policy depend on whether the S3 bucket is in your account or in another account\.

### Allow users of a bucket in a different account to decrypt logs<a name="create-kms-key-policy-for-cloudtrail-decrypt-different-bucket"></a>

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
      "Resource": "arn:aws:kms:us-east-1:111111111111:key/keyA"
    }
  ]
}
```

### Allow users in a different account to decrypt logs from your bucket<a name="create-kms-key-policy-for-cloudtrail-decrypt-same-bucket"></a>

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

For steps on editing a CMK policy for use with CloudTrail, see [Editing a Key Policy](http://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.