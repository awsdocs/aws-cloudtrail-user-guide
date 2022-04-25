# Controlling user permissions for CloudTrail<a name="control-user-permissions-for-cloudtrail"></a>

AWS CloudTrail integrates with AWS Identity and Access Management \(IAM\), which allows you to control access to CloudTrail and to other AWS resources that CloudTrail requires, including Amazon S3 buckets and Amazon Simple Notification Service \(Amazon SNS\) topics\. You can use AWS Identity and Access Management to control which AWS users can create, configure, or delete AWS CloudTrail trails, start and stop logging, and access the buckets that contain log information\. To learn more, see [Identity and Access Management for AWS CloudTrail](security-iam.md)\.

The following topics might also help you understand permissions, policies, and CloudTrail security:
+ [Amazon S3 bucket naming requirements](cloudtrail-s3-bucket-naming-requirements.md)
+ [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)
+  An example of a bucket policy for an organization trail in [Creating a trail for an organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.
+ [Amazon SNS topic policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)
+ [Encrypting CloudTrail log files with AWS KMS–managed keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)
+  [Default KMS key policy created in CloudTrail console](default-kms-key-policy.md)
+ [Granting permission to view AWS Config information on the CloudTrail console](security_iam_id-based-policy-examples.md#grant-aws-config-permissions-for-cloudtrail-users)
+ [Sharing CloudTrail log files between AWS accounts](cloudtrail-sharing-logs.md)
+ [Required permissions for creating an organization trail](creating-an-organizational-trail-prepare.md#org_trail_permissions)
+ [Using a previously\-existing IAM role to add monitoring of an organization trail to Amazon CloudWatch Logs](creating-an-organizational-trail-prepare.md#cwl-org-pb)