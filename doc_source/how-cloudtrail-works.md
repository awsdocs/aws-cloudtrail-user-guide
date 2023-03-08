# How CloudTrail works<a name="how-cloudtrail-works"></a>

CloudTrail is enabled on your AWS account when you create it\. When activity occurs in your AWS account, that activity is recorded in a CloudTrail event\. You can easily view events in the CloudTrail console by going to **Event history**\. 

Event history allows you to view, search, and download the past 90 days of activity in your AWS account\. In addition, you can create a CloudTrail trail to archive, analyze, and respond to changes in your AWS resources\. A trail is a configuration that enables delivery of events to an Amazon S3 bucket that you specify\. You can also deliver and analyze events in a trail with Amazon CloudWatch Logs and Amazon EventBridge\. You can create a trail with the CloudTrail console, the AWS CLI, or the CloudTrail API\.

You can create two types of trails for an AWS account:

** A trail that applies to all Regions **  
When you create a trail that applies to all AWS Regions, CloudTrail records events in each AWS Region and delivers the CloudTrail event log files to an S3 bucket that you specify\. If a new AWS Region is added after you create a trail that applies to all AWS Regions, that new Region is automatically included, and events in that Region are logged\. Creating a multi\-Region trail is a recommended best practice since you capture activity in all Regions in your account\. All trails you create using the CloudTrail console are multi\-Region\. You can update a single\-Region trail to log all Regions by using the AWS CLI\. For more information, see [Creating a trail in the console \(basic event selectors\)](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console) and [Converting a trail that applies to one Region to apply to all Regions](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert)\.

** A trail that applies to one Region **  
When you create a trail that applies to one Region, CloudTrail records the events in that Region only\. It then delivers the CloudTrail event log files to an Amazon S3 bucket that you specify\. You can only create a single\-Region trail by using the AWS CLI\. If you create additional single trails, you can have those trails deliver CloudTrail event log files to the same Amazon S3 bucket or to separate buckets\. This is the default option when you create a trail using the AWS CLI or the CloudTrail API\. For more information, see [Creating, updating, and managing trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

**Note**  
For both types of trails, you can specify an Amazon S3 bucket from any Region\.

Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.

If you have created an organization in AWS Organizations, you can create an *organization trail* or an *organization event data store* that logs all events for all AWS accounts in that organization\. Organization trails or event data stores can apply to all AWS Regions, or the current Region\. Organization trails and event data stores must be created in the management account or delegated administrator account, and when specified as applying to an organization, are automatically applied to all member accounts in the organization\. Member accounts can see the organization trail or event data store, but cannot modify or delete it\. By default, member accounts do not have access to the log files for an organization trail in the Amazon S3 bucket, nor can they run queries on organization event data stores\.

You can change the configuration of a trail after you create it, including whether it logs events in one Region or all Regions\. To change a single\-Region trail to an all\-Region trail, or vice\-versa, you must run the AWS CLI [update\-trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md) command\. You can also change whether it logs data or CloudTrail Insights events\. Changing whether a trail logs events in one Region or in all Regions affects which events are logged\. For more information, see [Managing trails with the AWS CLI](cloudtrail-additional-cli-commands.md) \(AWS CLI\), and [Working with CloudTrail log files](cloudtrail-working-with-log-files.md)\.

By default, CloudTrail event log files are encrypted using Amazon S3 server\-side encryption \(SSE\)\. You can also choose to encrypt your log files with an AWS Key Management Service \(AWS KMS\) key\. You can store your log files in your bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications\.

CloudTrail publishes log files multiple times an hour, about every 5 minutes\. These log files contain API calls from services in the account that support CloudTrail\. For more information, see [CloudTrail supported services and integrations](cloudtrail-aws-service-specific-topics.md)\.

**Note**  
CloudTrail typically delivers logs within an average of about 5 minutes of an API call\. This time is not guaranteed\. Review the [AWS CloudTrail Service Level Agreement](http://aws.amazon.com/cloudtrail/sla) for more information\.  
CloudTrail captures actions made directly by the user or on behalf of the user by an AWS service\. For example, an AWS CloudFormation `CreateStack` call can result in additional API calls to Amazon EC2, Amazon RDS, Amazon EBS, or other services as required by the AWS CloudFormation template\. This behavior is normal and expected\. You can identify if the action was taken by an AWS service with the `invokedby` field in the CloudTrail event\.

To get started with CloudTrail, see [Getting started with AWS CloudTrail tutorial](cloudtrail-tutorial.md)\.

For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. For Amazon S3 and Amazon SNS pricing, see [Amazon S3 Pricing ](https://aws.amazon.com/s3/pricing/) and [Amazon SNS Pricing](https://aws.amazon.com/sns/pricing/)\.

**Topics**
+ [Event history](#how-cloudtrail-works-eventhistory)
+ [CloudTrail Lake and event data stores](#how-cloudtrail-works-lake)
+ [CloudTrail trails](#how-cloudtrail-works-trails)
+ [CloudTrail channels](#how-cloudtrail-works-channels)

## Event history<a name="how-cloudtrail-works-eventhistory"></a>

You can easily view events in the CloudTrail console by going to **Event history**\. Event history allows you to view, search, and download the past 90 days of management event activity in your AWS account\. You can search events in **Event history** by filtering for events on a single attribute\.

## CloudTrail Lake and event data stores<a name="how-cloudtrail-works-lake"></a>

You can create a CloudTrail Lake event data store to archive, analyze, and respond to changes in your AWS resources\. Events are aggregated into *event data stores*, which are immutable collections of events based on criteria that you select by applying [advanced event selectors](logging-data-events-with-cloudtrail.md#creating-data-event-selectors-advanced)\. You can keep event data in an event data store for up to seven years, or 2557 days\. CloudTrail Lake event data stores let you log events from applications outside of AWS, including from any source in your hybrid environments, such as in\-house or SaaS applications hosted on\-premises or in the cloud, virtual machines, or containers\. CloudTrail Lake lets you create integrations with more than 12 partners to log events that occur outside AWS to your event data stores\. To create an integration, you first configure a *channel* over which events are delivered\. You can use CloudTrail Lake to store, access, analyze, troubleshoot and take action on this data without maintaining multiple log aggregators and reporting tools\. For more information about how to get started with CloudTrail Lake, see [Working with AWS CloudTrail Lake](cloudtrail-lake.md) in this guide\.

Event data stores can log events from the current AWS Region, or from all Regions in your AWS account\. Event data stores that you are using to log **Integration** events from outside AWS must be for a single Region only; they cannot be multi\-Region event data stores\.

To change an event data store after you create it, you can run the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/update-event-data-store.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/update-event-data-store.html) command, or use the CloudTrail Lake console\.

If you have created an organization in AWS Organizations, you can create an *organization event data store* that logs all events for all AWS accounts in that organization\. Organization event data stores can apply to all AWS Regions, or the current Region\. Organization event data stores must be created in the management account or delegated administrator account, and when specified as applying to an organization, are automatically applied to all member accounts in the organization\. Member accounts cannot see the organization event data store, nor can they modify or delete it\. By default, member accounts do not have access to the log files for an organization event data store, nor can they run queries on organization event data stores\. Organization event data stores cannot be used to collect events from outside of AWS\.

## CloudTrail trails<a name="how-cloudtrail-works-trails"></a>

You can also create a CloudTrail trail to archive, analyze, and respond to changes in your AWS resources\. 

A *trail* is a configuration that enables delivery of events to an Amazon S3 bucket that you specify\. You can also deliver and analyze events in a trail with Amazon CloudWatch Logs and Amazon CloudWatch Events\. You can create trails with the CloudTrail console, the AWS CLI, or the CloudTrail API\.

You can create two types of trails for an AWS account:

** A trail that applies to all Regions **  
When you create a trail that applies to all Regions, CloudTrail records events in each Region and delivers the CloudTrail event log files to an S3 bucket that you specify\. If a Region is added after you create a trail that applies to all Regions, that new Region is automatically included, and events in that Region are logged\. Creating a multi\-Region trail is a recommended best practice since you capture activity in all Regions in your account\. All trails you create using the CloudTrail console are multi\-Region\. You can update a single\-Region trail to log all Regions by using the AWS CLI\. For more information, see [Creating a trail in the console \(basic event selectors\)](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console) and [Converting a trail that applies to one Region to apply to all Regions](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert)\.

** A trail that applies to one Region **  
When you create a trail that applies to one Region, CloudTrail records the events in that Region only\. It then delivers the CloudTrail event log files to an Amazon S3 bucket that you specify\. You can only create a single\-Region trail by using the AWS CLI\. If you create additional single trails, you can have those trails deliver CloudTrail event log files to the same Amazon S3 bucket or to separate buckets\. This is the default option when you create a trail using the AWS CLI or the CloudTrail API\. For more information, see [Creating, updating, and managing trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

**Note**  
For both types of trails, you can specify an Amazon S3 bucket from any Region\.

Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.

If you have created an organization in AWS Organizations, you can create an *organization trail* that logs all events for all AWS accounts in that organization\. Organization trails or event data stores can apply to all AWS Regions, or the current Region\. Organization trails must be created in the management account, and when specified as applying to an organization, are automatically applied to all member accounts in the organization\. Member accounts can see the organization trail , but cannot modify or delete it\. By default, member accounts do not have access to the log files for an organization trail in the Amazon S3 bucket\.

You can change the configuration of a trail after you create it, including whether it logs events in one Region or all Regions\. To change a single\-Region trail to an all\-Region trail, or vice\-versa, you must run the AWS CLI [update\-trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md) command\. You can also change whether it logs data or CloudTrail Insights events\. Changing whether a trail logs events in one Region or in all Regions affects which events are logged\. For more information, see [Managing trails with the AWS CLI](cloudtrail-additional-cli-commands.md) \(AWS CLI\), and [Working with CloudTrail log files](cloudtrail-working-with-log-files.md)\.

By default, CloudTrail event log files from trails are encrypted using Amazon S3 server\-side encryption \(SSE\)\. You can also choose to encrypt your log files with an AWS Key Management Service \(AWS KMS\) key\. You can store your log files in your bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications\.

## CloudTrail channels<a name="how-cloudtrail-works-channels"></a>

CloudTrail supports two types of *channels*:

**Channels for CloudTrail Lake integrations with event sources outside of AWS**  
CloudTrail Lake uses *channels* to bring non\-AWS events into CloudTrail Lake from external partners that work with CloudTrail, or from your own sources\. When you create a channel, you choose one or more event data stores to store events that arrive from the channel source\. You can change the destination event data stores for a channel as needed, as long as the destination event data stores are set to log activity events\. When you create a channel for events from an external partner, you provide a channel ARN to the partner or source application\. The resource policy attached to the channel allows the source to transmit events through the channel\. For more information, see [Create an integration with an event source outside of AWS](query-event-data-store-integration.md) and [https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateChannel.html](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateChannel.html) in the *AWS CloudTrail API Reference*\.

**Service\-linked channels**  
AWS services can create a service\-linked channel to receive CloudTrail events on your behalf\. The AWS service creating the service\-linked channel configures advanced event selectors for the channel and specifies whether the channel applies to all Regions, or a single Region\.  
Using the AWS CLI, you can view information about any CloudTrail service\-linked channels created by AWS services\. For more information, see [Viewing service\-linked channels for CloudTrail by using the AWS CLI](viewing-service-linked-channels.md)\.