# Controlling User Permissions for CloudTrail<a name="control-user-permissions-for-cloudtrail"></a>

AWS CloudTrail integrates with AWS Identity and Access Management \(IAM\), which allows you to control access to CloudTrail and to other AWS resources that CloudTrail requires, including Amazon S3 buckets and Amazon Simple Notification Service \(Amazon SNS\) topics\. You can use AWS Identity and Access Management to control which AWS users can create, configure, or delete AWS CloudTrail trails, start and stop logging, and access the buckets that contain log information\. To learn more, see [Identity and Access Management for AWS CloudTrail](security-iam.md)\.

The following topics might also help you understand permissions, policies, and CloudTrail security:
+ [Amazon S3 Bucket Naming Requirements](cloudtrail-s3-bucket-naming-requirements.md)
+ [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)
+  An example of a bucket policy for an organization trail in [Creating a Trail for an Organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.
+ [Amazon SNS Topic Policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)
+ [Encrypting CloudTrail Log Files with AWS KMS–Managed Keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)
+  [Default Key Policy Created in CloudTrail Console](default-cmk-policy.md)
+ [Granting Permission to View AWS Config Information on the CloudTrail Console](security_iam_id-based-policy-examples.md#grant-aws-config-permissions-for-cloudtrail-users)
+ [Sharing CloudTrail Log Files Between AWS Accounts](cloudtrail-sharing-logs.md)
+ [Required permissions for creating an organization trail](creating-an-organizational-trail-prepare.md#org_trail_permissions)
+ [Using a previously\-existing IAM role to add monitoring of an organization trail to Amazon CloudWatch Logs](creating-an-organizational-trail-prepare.md#cwl-org-pb)