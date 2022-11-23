# Prepare for creating a trail for your organization<a name="creating-an-organizational-trail-prepare"></a>

Before you create a trail for your organization, be sure that your organization management account or delegated administrator account is set up correctly for trail creation\.
+ Your organization must have all features enabled before you can create a trail for it\. For more information, see [Enabling All Features in Your Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_support-all-features.html)\.
+ The management account or delegated administrator account must have the **AWSServiceRoleForOrganizations** role\. This role is created automatically by Organizations when you create your organization, and is required for CloudTrail to log events for an organization\. For more information, see [Organizations and service\-linked roles](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_integrate_services.html#orgs_integrate_services-using_slrs)\.
+ The IAM user or role that will be used to create the organization trail in the management account or delegated administrator account must have sufficient permissions to create an organization trail\. At a minimum, to create an organization trail, you must have the **AWSCloudTrail\_FullAccess** policy or equivalent permissions applied\. You must also have sufficient permissions in IAM and Organizations to create the service\-linked role and enable trusted access\. The following example policy shows the minimum required permissions\.
**Note**  
The **AWSCloudTrail\_FullAccess** policy is not intended to be shared broadly across your AWS account\. Instead, it should be restricted to AWS account administrators due to the highly sensitive information collected by CloudTrail\. Users with this role have the ability to disable or reconfigure the most sensitive and important auditing functions in their AWS accounts\. For this reason, access to this policy should be closely controlled and monitored\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "iam:GetRole",
                  "organizations:EnableAWSServiceAccess",
                  "organizations:ListAccounts",
                  "iam:CreateServiceLinkedRole",
                  "organizations:DisableAWSServiceAccess",
                  "organizations:DescribeOrganization",
                  "organizations:ListAWSServiceAccessForOrganization"
              ],
              "Resource": "*"
          }
      ]
  }
  ```
+ To use the AWS CLI or the CloudTrail APIs to create an organization trail, you must enable trusted access for CloudTrail in Organizations, and you must manually create an Amazon S3 bucket with a policy that allows logging for an organization trail\. For more information, see [Creating a trail for an organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.
+ To use an existing IAM role to add monitoring of an organization trail to Amazon CloudWatch Logs, you must manually modify the IAM role to allow delivery of CloudWatch Logs for member accounts to the CloudWatch Logs group for the management or delegated administrator account, as shown in the following example\.
**Note**  
You must use an IAM role and CloudWatch Logs log group that exists in your own account\. You cannot use an IAM role or CloudWatch Logs log group owned by a different account\. 

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "AWSCloudTrailCreateLogStream20141101",
              "Effect": "Allow",
              "Action": [
                  "logs:CreateLogStream"
              ],
              "Resource": [
                  "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:111111111111_CloudTrail_us-east-2*",
                  "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:o-exampleorgid_*"
              ]
          },
          {
              "Sid": "AWSCloudTrailPutLogEvents20141101",
              "Effect": "Allow",
              "Action": [
                  "logs:PutLogEvents"
              ],
              "Resource": [
                  "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:111111111111_CloudTrail_us-east-2*",             
                  "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:o-exampleorgid_*"
              ]
          }
      ]
  }
  ```

  You can learn more about CloudTrail and Amazon CloudWatch Logs in [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\. In addition, consider the limits on CloudWatch Logs and the pricing considerations for the service before deciding to enable the experience for an organization trail\. For more information, see [CloudWatch Logs Limits](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html) and [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.
+ To log data events in your organization trail for specific resources in member accounts, have ready a list of Amazon Resource Names \(ARNs\) for each of those resources\. Member account resources are not displayed in the CloudTrail console when you create a trail; you can browse for resources in the management account on which data event collection is supported, such as S3 buckets\. Similarly, if you want to add specific member resources when creating or updating an organization trail at the command line, you need the ARNs for those resources\.
**Note**  
Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

You should also consider reviewing how many trails already exist in the management account, delegated administrator account, and in the member accounts before creating an organization trail\. CloudTrail limits the number of trails that can be created in each region\. You cannot exceed this limit in the region where you create the organization trail in the management account or delegated administrator account\. However, the trail will be created in the member accounts even if member accounts have reached the limit of trails in a region\. While the first trail of management events in any region is free, charges apply to additional trails\. To reduce the potential cost of an organization trail, consider deleting any unneeded trails in the management and member accounts\. For more information about CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

## Security best practices in organization trails<a name="organizational-trail-prepare-confused-deputy"></a>

As a security best practice, we recommend adding the `aws:SourceArn` condition key to resource policies \(such as those for S3 buckets, KMS keys, or SNS topics\) that you use with an organization trail\. The value of `aws:SourceArn` is the organization trail ARN \(or ARNs, if you are using the same resource for more than one trail, such as the same S3 bucket to store logs for more than one trail\)\. This ensures that the resource, such as an S3 bucket, accepts only data that is associated with the specific trail\. The trail ARN must use the account ID of the management account\. The following policy snippet shows an example where more than one trail is using the resource\.

```
"Condition": {
    "StringEquals": {
      "AWS:SourceArn": ["Trail_ARN_1",..., "Trail_ARN_n"]
    }
}
```

For information about how to add condition keys to resource policies, see the following:
+ [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)
+ [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)
+ [Amazon SNS topic policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)