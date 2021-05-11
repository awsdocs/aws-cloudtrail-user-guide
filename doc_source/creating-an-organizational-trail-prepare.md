# Prepare for creating a trail for your organization<a name="creating-an-organizational-trail-prepare"></a>

Before you create a trail for your organization, be sure that both your organization and your management account are set up correctly for trail creation\.
+ Your organization must have all features enabled before you can create a trail for it\. For more information, see [Enabling All Features in Your Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_support-all-features.html)\.
+ The management account must have the **AWSServiceRoleForOrganizations** role\. This role is created automatically by Organizations when you create your organization, and is required for CloudTrail to log events for an organization\. For more information, see [Organizations and Service\-Linked Roles](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_integrate_services.html#orgs_integrate_services-using_slrs)\.
+ The IAM user or role that will be used to create the organization trail in the management account must have sufficient permissions to create an organization trail\. At a minimum, to create an organization trail, you must have the **AWSCloudTrail\_FullAccess** policy or equivalent permissions applied\. You must also have sufficient permissions in IAM and Organizations to create the service\-linked role and enable trusted access\. The following example policy shows the minimum required permissions\.
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
+ To use an existing IAM role to add monitoring of an organization trail to Amazon CloudWatch Logs, you must manually modify the IAM role to allow delivery of CloudWatch Logs for member accounts to the CloudWatch Logs group in the management account, as shown in the following example\.

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
+ To log data events in your organization trail for specific Amazon S3 buckets, S3 object\-level API activity on AWS Outposts objects, Managed Blockchain nodes, S3 Object on Lambda access points, or AWS Lambda functions in member accounts, have ready a list of Amazon Resource Names \(ARNs\) for each of those resources\. Member account resources are not displayed in the CloudTrail console when you create a trail; you can browse for resources in the management account on which data event collection is supported, such as S3 buckets\. Similarly, if you want to add specific member resources when creating or updating an organization trail at the command line, you will need the ARNs for those resources\.
**Note**  
Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

You should also consider reviewing how many trails already exist in the management account and in the member accounts before creating an organization trail\. CloudTrail limits the number of trails that can be created in each region\. You cannot exceed this limit in the region where you create the organization trail in the management account\. However, the trail will be created in the member accounts even if member accounts have reached the limit of trails in a region\. While the first trail in any region is free, charges apply to additional trails\. To reduce the potential cost of an organization trail, consider deleting any unneeded trails in the management and member accounts\. For more information about CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.