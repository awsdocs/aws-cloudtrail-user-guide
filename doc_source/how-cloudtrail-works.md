# How CloudTrail Works<a name="how-cloudtrail-works"></a>

CloudTrail is enabled on your AWS account when you create it\. When activity occurs in your AWS account, that activity is recorded in a CloudTrail event\. You can easily view events in the CloudTrail console by going to **Event history**\. 

 Event history allows you to view, search, and download the past 90 days of  activity in your AWS account\. In addition, you can create a CloudTrail trail to archive, analyze, and respond to changes in your AWS resources\. A trail is a configuration that enables delivery of events to an Amazon S3 bucket that you specify\. You can also deliver and analyze events in a trail with Amazon CloudWatch Logs and Amazon CloudWatch Events\. You can create a trail with the CloudTrail console, the AWS CLI, or the CloudTrail API\. 

You can create two types of trails:

** A trail that applies to all regions **  
When you create a trail that applies to all regions, CloudTrail records events in each region and delivers the CloudTrail event log files to an S3 bucket that you specify\. If a region is added after you create a trail that applies to all regions, that new region is automatically included, and events in that region are logged\. This is the default option when you create a trail in the CloudTrail console\. For more information, see [Creating a Trail in the Console](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console)\.

** A trail that applies to one region **  
When you create a trail that applies to one region, CloudTrail records the events in that region only\. It then delivers the CloudTrail event log files to an Amazon S3 bucket that you specify\. If you create additional single trails, you can have those trails deliver CloudTrail event log files to the same Amazon S3 bucket or to separate buckets\. This is the default option when you create a trail using the AWS CLI or the CloudTrail API\. For more information, see [Creating a Trail with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

**Note**  
For both types of trails, you can specify an Amazon S3 bucket from any region\.

You can change the configuration of a trail after you create it, including whether it logs events in one region or all regions\. You can also change whether it logs data events\. Changing whether a trail logs events in one region or in all regions affects which events are logged\. For more information, see [Updating a Trail](cloudtrail-update-a-trail-console.md) \(console\), [Managing Trails](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md#cloudtrail-additional-cli-commands) \(AWS CLI\), and [Logging Data and Management Events for Trails](logging-management-and-data-events-with-cloudtrail.md)\.

By default, CloudTrail event log files are encrypted using Amazon S3 server\-side encryption \(SSE\)\. You can also choose to encrypt your log files with an AWS Key Management Service \(AWS KMS\) key\. You can store your log files in your bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications\.

CloudTrail typically delivers log files within 15 minutes of account activity\. In addition, CloudTrail publishes log files multiple times an hour, about every five minutes\. These log files contain API calls from services in the account that support CloudTrail\. For more information, see [CloudTrail Supported Services and Integrations](cloudtrail-aws-service-specific-topics.md)\.

**Note**  
CloudTrail captures actions made directly by the user or on behalf of the user by an AWS service\. For example, an AWS CloudFormation `CreateStack` call can result in additional API calls to Amazon EC2, Amazon RDS, Amazon EBS, or other services as required by the AWS CloudFormation template\. This behavior is normal and expected\. You can identify if the action was taken by an AWS service with the `invokedby` field in the CloudTrail event\.

To get started with CloudTrail, see [Getting Started with CloudTrail](cloudtrail-getting-started.md)\.

For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. For Amazon S3 and Amazon SNS pricing, see [Amazon S3 Pricing ](https://aws.amazon.com/s3/pricing/) and [Amazon SNS Pricing](https://aws.amazon.com/sns/pricing/)\.