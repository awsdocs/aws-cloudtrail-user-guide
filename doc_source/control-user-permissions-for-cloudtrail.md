# Controlling User Permissions for CloudTrail<a name="control-user-permissions-for-cloudtrail"></a>

AWS CloudTrail integrates with AWS Identity and Access Management \(IAM\), which allows you to control access to CloudTrail and to other AWS resources that CloudTrail requires, including Amazon S3 buckets and Amazon Simple Notification Service \(Amazon SNS\) topics\. You can use AWS Identity and Access Management to control which AWS users can create, configure, or delete AWS CloudTrail trails, start and stop logging, and access the buckets that contain log information\. 

If you work with CloudTrail as the root user in your account, you can perform all the tasks associated with trails, including creating trails, reading logs, and so on\. If other people in your organization need to work with CloudTrail, you can create IAM users for those people and give them individual names and passwords\. When you do that, you must also give users permissions to work with CloudTrail and with any other AWS services they need to access, such as Amazon S3\. \(By default, IAM users have no permissions and cannot perform any actions in AWS\.\) 

**Important**  
We consider it a best practice not to use root account credentials to perform everyday work in AWS\. Instead, we recommend that you create an IAM administrators group with appropriate permissions, create IAM users for the people in your organization who need to perform administrative tasks \(including for yourself\), and add those users to the administrative group\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html) in the *IAM User Guide* guide\. 

The following topics might also help you understand permissions, policies, and CloudTrail security:
+ [Amazon S3 Bucket Naming Requirements](cloudtrail-s3-bucket-naming-requirements.md)
+ [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)
+  [Create or update an Amazon S3 bucket to use to store the log files for an organization trail](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md#cloudtrail-create-organizational-trail-by-using-the-cli-bucket)
+ [Amazon SNS Topic Policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)
+ [Encrypting CloudTrail Log Files with AWS KMS–Managed Keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)
+  [Default Key Policy Created in CloudTrail Console](default-cmk-policy.md)
+ [Granting Permission to View AWS Config Information on the CloudTrail Console](grant-custom-permissions-for-cloudtrail-users.md#grant-aws-config-permissions-for-cloudtrail-users)
+ [Sharing CloudTrail Log Files Between AWS Accounts](cloudtrail-sharing-logs.md)
+ [Required permissions for creating an organization trail](creating-an-organizational-trail-prepare.md#org_trail_permissions)
+ [Using a previously\-existing IAM role to add monitoring of an organization trail to Amazon CloudWatch Logs](creating-an-organizational-trail-prepare.md#cwl-org-pb)

**Topics**
+ [Granting Permissions for CloudTrail Administration](grant-permissions-for-cloudtrail-administration.md)
+ [Granting Custom Permissions for CloudTrail Users](grant-custom-permissions-for-cloudtrail-users.md)