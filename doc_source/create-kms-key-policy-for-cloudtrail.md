# AWS KMS Key Policy for CloudTrail<a name="create-kms-key-policy-for-cloudtrail"></a>

You can create a customer master key \(CMK\) in three ways:

+ The CloudTrail console

+ The IAM console

+ The AWS CLI

If you create a CMK in the CloudTrail console, CloudTrail adds the required CMK policy sections for you\. You do not need to complete the following steps\.

If you create a CMK in the IAM console or the AWS CLI, you need to add policy sections to the key so that you can use it with CloudTrail\. The policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\.

See the following resources:

+ To create a CMK with the AWS CLI, see [create\-key](http://docs.aws.amazon.com/cli/latest/reference/kms/create-key.html)\. 

+ To edit a CMK policy for CloudTrail, see [Editing a Key Policy](http://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-editing) in the *AWS Key Management Service Developer Guide*\.

+ For technical details on how CloudTrail uses AWS KMS, see [How AWS CloudTrail Uses AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/services-cloudtrail.html) in the *AWS Key Management Service Developer Guide*\.