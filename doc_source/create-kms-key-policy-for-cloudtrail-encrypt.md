# Granting encrypt permissions<a name="create-kms-key-policy-for-cloudtrail-encrypt"></a>

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

For steps on editing a CMK policy for use with CloudTrail, see [Editing a Key Policy](http://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the AWS Key Management Service Developer Guide\.