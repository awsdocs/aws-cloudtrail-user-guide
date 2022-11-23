# How CloudTrail works<a name="how-cloudtrail-works"></a>

CloudTrail is enabled on your AWS account when you create it\. When activity occurs in your AWS account, that activity is recorded in a CloudTrail event\. You can easily view events in the CloudTrail console by going to **Event history**\. 

Event history allows you to view, search, and download the past 90 days of activity in your AWS account\. In addition, you can create a CloudTrail trail to archive, analyze, and respond to changes in your AWS resources\. A trail is a configuration that enables delivery of events to an Amazon S3 bucket that you specify\. You can also deliver and analyze events in a trail with Amazon CloudWatch Logs and Amazon EventBridge\. You can create a trail with the CloudTrail console, the AWS CLI, or the CloudTrail API\.

You can create two types of trails for an AWS account:

** A trail that applies to all regions **  
When you create a trail that applies to all regions, CloudTrail records events in each region and delivers the CloudTrail event log files to an S3 bucket that you specify\. If a region is added after you create a trail that applies to all regions, that new region is automatically included, and events in that region are logged\. Because creating a trail in all regions is a recommended best practice, so you capture activity in all regions in your account, an all\-regions trail is the default option when you create a trail in the CloudTrail console\. You can only update a single\-region trail to log all regions by using the AWS CLI\. For more information, see [Creating a trail in the console \(basic event selectors\)](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console)\.

** A trail that applies to one region **  
When you create a trail that applies to one region, CloudTrail records the events in that region only\. It then delivers the CloudTrail event log files to an Amazon S3 bucket that you specify\. You can only create a single\-region trail by using the AWS CLI\. If you create additional single trails, you can have those trails deliver CloudTrail event log files to the same Amazon S3 bucket or to separate buckets\. This is the default option when you create a trail using the AWS CLI or the CloudTrail API\. For more information, see [Creating, updating, and managing trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

**Note**  
For both types of trails, you can specify an Amazon S3 bucket from any region\.

Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.

If you have created an organization in AWS Organizations, you can also create a trail that will log all events for all AWS accounts in that organization\. This is referred to as an *organization trail*\. Organization trails can apply to all AWS Regions or one Region\. Organization trails must be created in the management account or delegated administrator account, and when specified as applying to an organization, are automatically applied to all member accounts in the organization\. Member accounts will be able to see the organization trail, but cannot modify or delete it\. By default, member accounts will not have access to the log files for the organization trail in the Amazon S3 bucket\.

You can change the configuration of a trail after you create it, including whether it logs events in one region or all regions\. To change a single\-region trail to an all\-region trail, or vice\-versa, you must run the AWS CLI [update\-trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md) command\. You can also change whether it logs data or CloudTrail Insights events\. Changing whether a trail logs events in one region or in all regions affects which events are logged\. For more information, see [Managing trails with the AWS CLI](cloudtrail-additional-cli-commands.md) \(AWS CLI\), and [Working with CloudTrail log files](cloudtrail-working-with-log-files.md)\.

By default, CloudTrail event log files are encrypted using Amazon S3 server\-side encryption \(SSE\)\. You can also choose to encrypt your log files with an AWS Key Management Service \(AWS KMS\) key\. You can store your log files in your bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications\.

CloudTrail publishes log files multiple times an hour, about every five minutes\. These log files contain API calls from services in the account that support CloudTrail\. For more information, see [CloudTrail supported services and integrations](cloudtrail-aws-service-specific-topics.md)\.

**Note**  
CloudTrail typically delivers logs within an average of about 15 minutes of an API call\. This time is not guaranteed\. Review the [AWS CloudTrail Service Level Agreement](http://aws.amazon.com/cloudtrail/sla) for more information\.  
CloudTrail captures actions made directly by the user or on behalf of the user by an AWS service\. For example, an AWS CloudFormation `CreateStack` call can result in additional API calls to Amazon EC2, Amazon RDS, Amazon EBS, or other services as required by the AWS CloudFormation template\. This behavior is normal and expected\. You can identify if the action was taken by an AWS service with the `invokedby` field in the CloudTrail event\.

To get started with CloudTrail, see [Getting started with AWS CloudTrail tutorial](cloudtrail-tutorial.md)\.

For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. For Amazon S3 and Amazon SNS pricing, see [Amazon S3 Pricing ](https://aws.amazon.com/s3/pricing/) and [Amazon SNS Pricing](https://aws.amazon.com/sns/pricing/)\.