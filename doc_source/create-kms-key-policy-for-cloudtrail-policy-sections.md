# Required CMK policy sections for use with CloudTrail<a name="create-kms-key-policy-for-cloudtrail-policy-sections"></a>

If you created a CMK in the CloudTrail console, CloudTrail adds the required CMK policy for you\. You do not need to manually add the policy statements\. See [Default Key Policy Created in CloudTrail Console](default-cmk-policy.md)\.

If you created a CMK with the IAM console or the AWS CLI, then you must, at minimum, add three statements to your CMK policy for it to work with CloudTrail\.

1. Enable CloudTrail log encrypt permissions\. See [Granting encrypt permissions](create-kms-key-policy-for-cloudtrail-encrypt.md)\.

1. Enable CloudTrail log decrypt permissions\. See [Granting decrypt permissions](create-kms-key-policy-for-cloudtrail-decrypt.md)\.

1. Enable CloudTrail to describe CMK properties\. See [Enable CloudTrail to describe CMK properties](create-kms-key-policy-for-cloudtrail-describe.md)\.

**Note**  
When you add the new sections to your CMK policy, do not change any existing sections in the policy\. 

**Warning**  
If encryption is enabled on a trail and the CMK is disabled or the CMK policy is not correctly configured for CloudTrail, CloudTrail will not deliver logs until the CMK issue is corrected\.