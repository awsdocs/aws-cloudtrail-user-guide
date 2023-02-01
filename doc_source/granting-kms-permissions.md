# Granting permissions to create a KMS key<a name="granting-kms-permissions"></a>

You can grant users permission to create an AWS KMS key with the `AWSKeyManagementServicePowerUser` policy\.

**To grant permission to create a KMS key**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose the group or user that you want to give permission\.

1.  Choose **Permissions**, and then choose **Attach Policy**\. 

1. Search for **AWSKeyManagementServicePowerUser**, choose the policy, and then choose **Attach policy**\. 

   The user now has permission to create a KMS key\. If you want to create custom policies for your users, see [Creating Customer Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html#create-managed-policy-console) in the *IAM User Guide*\.