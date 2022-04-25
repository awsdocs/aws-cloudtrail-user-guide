# Troubleshooting AWS CloudTrail identity and access<a name="security_iam_troubleshoot"></a>

Use the following information to help you diagnose and fix common issues that you might encounter when working with CloudTrail and IAM\.

**Topics**
+ [I am not authorized to perform an action in CloudTrail](#security_iam_troubleshoot-no-permissions)
+ [I'm an Administrator and Want to Allow Others to Access CloudTrail](#security_iam_troubleshoot-admin-delegate)
+ [I want to allow people outside of my AWS account to access my CloudTrail resources](#security_iam_troubleshoot-cross-account-access)

## I am not authorized to perform an action in CloudTrail<a name="security_iam_troubleshoot-no-permissions"></a>

If the AWS Management Console tells you that you're not authorized to perform an action, then you must contact your administrator for assistance\. Your administrator is the person that provided you with your user name and password\.

The following example error occurs when the `mateojackson` IAM user tries to use the console to view details about a trail but does not have either the appropriate CloudTrail managed policy \(**AWSCloudTrail\_FullAccess** or **AWSCloudTrailReadOnlyAccess**\) or the equivalent permissions applied to his account\.

```
User: arn:aws:iam::123456789012:user/mateojackson is not authorized to perform: cloudtrail:GetTrailStatus on resource: My-Trail
```

In this case, Mateo asks his administrator to update his policies to allow him to access trail information and status in the console\.

If you are signed in with an IAM user or role that has the **AWSCloudTrail\_FullAccess** managed policy or its equivalent permissions, and you cannot configure AWS Config or Amazon CloudWatch Logs integration with a trail, you might be missing the required permissions for integration with those services\. For more information, see [Granting permission to view AWS Config information on the CloudTrail console](security_iam_id-based-policy-examples.md#grant-aws-config-permissions-for-cloudtrail-users) and [Granting permission to view and configure Amazon CloudWatch Logs information on the CloudTrail console](security_iam_id-based-policy-examples.md#grant-cloudwatch-permissions-for-cloudtrail-users)\.

## I'm an Administrator and Want to Allow Others to Access CloudTrail<a name="security_iam_troubleshoot-admin-delegate"></a>

To allow others to access CloudTrail, you must create an IAM entity \(user, group, or role\) for the person or application that needs access\. They will use the credentials for that entity to access AWS\. You must then attach a policy to the entity that grants them the correct permissions in CloudTrail\. For examples of how to do this, see [Granting custom permissions for CloudTrail users](security_iam_id-based-policy-examples.md#grant-custom-permissions-for-cloudtrail-users) and [Granting permissions for CloudTrail administration](security_iam_id-based-policy-examples.md#grant-permissions-for-cloudtrail-administration)\.



## I want to allow people outside of my AWS account to access my CloudTrail resources<a name="security_iam_troubleshoot-cross-account-access"></a>

You can create a role and share CloudTrail information between multiple AWS accounts\. For more information, see [Sharing CloudTrail log files between AWS accounts](cloudtrail-sharing-logs.md)\.

You can create a role that users in other accounts or people outside of your organization can use to access your resources\. You can specify who is trusted to assume the role\. For services that support resource\-based policies or access control lists \(ACLs\), you can use those policies to grant people access to your resources\.

To learn more, consult the following:
+ To learn whether CloudTrail supports these features, see [How AWS CloudTrail works with IAM](security_iam_service-with-iam.md)\.
+ To learn how to provide access to your resources across AWS accounts that you own, see [Providing access to an IAM user in another AWS account that you own](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html) in the *IAM User Guide*\.
+ To learn how to provide access to your resources to third\-party AWS accounts, see [Providing access to AWS accounts owned by third parties](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_third-party.html) in the *IAM User Guide*\.
+ To learn how to provide access through identity federation, see [Providing access to externally authenticated users \(identity federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_federated-users.html) in the *IAM User Guide*\.
+ To learn the difference between using roles and resource\-based policies for cross\-account access, see [How IAM roles differ from resource\-based policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.