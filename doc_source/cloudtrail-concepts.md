# CloudTrail concepts<a name="cloudtrail-concepts"></a>

This section summarizes basic concepts related to CloudTrail\.

**Contents**
+ [What are CloudTrail events?](#cloudtrail-concepts-events)
  + [What are management events?](#cloudtrail-concepts-management-events)
  + [What are data events?](#cloudtrail-concepts-data-events)
  + [What are Insights events?](#cloudtrail-concepts-insights-events)
+ [What is CloudTrail event history?](#cloudtrail-concepts-event-history)
+ [What are trails?](#cloudtrail-concepts-trails)
+ [What are organization trails?](#cloudtrail-concepts-trails-org)
+ [How do you manage CloudTrail?](#cloudtrail-concepts-manage)
  + [CloudTrail console](#cloudtrail-concepts-console)
  + [CloudTrail CLI](#cloudtrail-concepts-cli)
  + [CloudTrail APIs](#cloudtrail-concepts-api)
  + [AWS SDKs](#cloudtrail-concepts-sdk)
  + [Why use tags for trails?](#cloudtrail-concepts-tags)
+ [How do you control access to CloudTrail?](#cloudtrail-concepts-iam)
+ [How do you log management and data events?](#understanding-event-selectors)
+ [How do you log CloudTrail Insights events?](#understanding-insight-selectors)
+ [How do you perform monitoring with CloudTrail?](#cloudtrail-concepts-monitoring)
  + [CloudWatch Logs, CloudWatch Events, and CloudTrail](#cloudtrail-concepts-cloudwatch-logs)
+ [How does CloudTrail behave regionally and globally?](#cloudtrail-concepts-regional-and-global-services)
  + [What are the advantages of applying a trail to all Regions?](#cloudtrail-concepts-trails-enable-all-regions-advantages)
  + [What happens when you apply a trail to all Regions?](#cloudtrail-concepts-trails-enable-all-regions)
  + [Multiple trails per Region](#cloudtrail-concepts-trails-multiple-trails-per-region)
  + [AWS Security Token Service \(AWS STS\) and CloudTrail](#cloudtrail-concepts-sts-regionalization)
+ [About global service events](#cloudtrail-concepts-global-service-events)
+ [How does CloudTrail relate to other AWS monitoring services?](#cloudtrail-concepts-other-aws-monitoring-services)
+ [Partner solutions](#cloudtrail-concepts-partner-solutions)

## What are CloudTrail events?<a name="cloudtrail-concepts-events"></a>

An event in CloudTrail is the record of an activity in an AWS account\. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail\. CloudTrail events provide a history of both API and non\-API account activity made through the AWS Management Console, AWS SDKs, command line tools, and other AWS services\. There are three types of events that can be logged in CloudTrail: management events, data events, and CloudTrail Insights events\. By default, trails log management events, but not data or Insights events\.

All event types use the same CloudTrail JSON log format\. 

**Note**  
CloudTrail does not log all AWS services and all events\. For more information about which APIs are logged for a specific service, see documentation for that service in [CloudTrail Supported Services and Integrations](cloudtrail-aws-service-specific-topics.md)\.

### What are management events?<a name="cloudtrail-concepts-management-events"></a>

Management events provide information about management operations that are performed on resources in your AWS account\. These are also known as *control plane operations*\. Example management events include:
+ Configuring security \(for example, IAM `AttachRolePolicy` API operations\)\.
+ Registering devices \(for example, Amazon EC2 `CreateDefaultVpc` API operations\)\.
+ Configuring rules for routing data \(for example, Amazon EC2 `CreateSubnet` API operations\)\.
+ Setting up logging \(for example, AWS CloudTrail `CreateTrail` API operations\)\.

Management events can also include non\-API events that occur in your account\. For example, when a user signs in to your account, CloudTrail logs the `ConsoleLogin` event\. For more information, see [Non\-API Events Captured by CloudTrail](cloudtrail-non-api-events.md)\. For a list of management events that CloudTrail logs for AWS services, see [CloudTrail Supported Services and Integrations](cloudtrail-aws-service-specific-topics.md)\.

### What are data events?<a name="cloudtrail-concepts-data-events"></a>

Data events provide information about the resource operations performed on or in a resource\. These are also known as *data plane operations*\. Data events are often high\-volume activities\. The following data types are recorded:
+ Amazon S3 object\-level API activity \(for example, `GetObject`, `DeleteObject`, and `PutObject` API operations\)\.
+ AWS Lambda function execution activity \(the `Invoke` API\)\.
+ Amazon DynamoDB object\-level API activity on tables \(for example, `PutItem`, `DeleteItem`, and `UpdateItem` API operations\)\.
+ Amazon S3 object\-level API activity on AWS Outposts\.
+ Amazon Managed Blockchain JSON\-RPC calls on Ethereum nodes, such as `eth_getBalance` or `eth_getBlockByNumber`\.
+ API activity on S3 Object Lambda access points, such as calls to `CompleteMultipartUpload` and `GetObject`\.

Data events are not logged by default when you create a trail\. To record CloudTrail data events, you must explicitly add to a trail the supported resources or resource types for which you want to collect activity\. For more information, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Data events](logging-data-events-with-cloudtrail.md#logging-data-events)\.

Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

### What are Insights events?<a name="cloudtrail-concepts-insights-events"></a>

CloudTrail Insights events capture unusual activity in your AWS account\. If you have Insights events enabled, and CloudTrail detects unusual activity, Insights events are logged to a different folder or prefix in the destination S3 bucket for your trail\. You can also see the type of insight and the incident time period when you view Insights events on the CloudTrail console\. Insights events provide relevant information, such as the associated API, incident time, and statistics, that help you understand and act on unusual activity\. Unlike other types of events captured in a CloudTrail trail, Insights events are logged only when CloudTrail detects changes in your account's API usage that differ significantly from the account's typical usage patterns\. Examples of activity that might generate Insights events include:
+ Your account typically logs no more than 20 Amazon S3 `deleteBucket` API calls per minute, but your account starts to log an average of 100 `deleteBucket` API calls per minute\. An Insights event is logged at the start of the unusual activity, and another Insights event is logged to mark the end of the unusual activity\.
+ Your account typically logs 20 calls per minute to the Amazon EC2 `AuthorizeSecurityGroupIngress` API, but your account starts to log zero calls to `AuthorizeSecurityGroupIngress`\. An Insights event is logged at the start of the unusual activity, and ten minutes later, when the unusual activity ends, another Insights event is logged to mark the end of the unusual activity\.

These examples are provided for illustration purposes only\. Your results may vary depending on your use case\.

Insights events are disabled by default when you create a trail\. To record CloudTrail Insights events, you must explicitly enable Insights event collection on a new or existing trail\. For more information, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Logging Insights Events for Trails](logging-insights-events-with-cloudtrail.md)\.

Additional charges apply for logging CloudTrail Insights events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

## What is CloudTrail event history?<a name="cloudtrail-concepts-event-history"></a>

CloudTrail event history provides a viewable, searchable, and downloadable record of the past 90 days of CloudTrail events\. You can use this history to gain visibility into actions taken in your AWS account in the AWS Management Console, AWS SDKs, command line tools, and other AWS services\. You can customize your view of event history in the CloudTrail console by selecting which columns are displayed\. For more information, see [Viewing Events with CloudTrail Event History](view-cloudtrail-events.md)\.

## What are trails?<a name="cloudtrail-concepts-trails"></a>

 A trail is a configuration that enables delivery of CloudTrail events to an Amazon S3 bucket, CloudWatch Logs, and CloudWatch Events\. You can use a trail to filter the CloudTrail events you want delivered, encrypt your CloudTrail event log files with an AWS KMS key, and set up Amazon SNS notifications for log file delivery\. For more information about how to create and manage a trail, see [Creating a trail for your AWS account](cloudtrail-create-and-update-a-trail.md)\.

## What are organization trails?<a name="cloudtrail-concepts-trails-org"></a>

 An organization trail is a configuration that enables delivery of CloudTrail events in the management account and all member accounts in an AWS Organizations organization to the same Amazon S3 bucket, CloudWatch Logs, and CloudWatch Events\. Creating an organization trail helps you define a uniform event logging strategy for your organization\. 

When you create an organization trail, a trail with the name that you give it will be created in every AWS account that belongs to your organization\. Users with CloudTrail permissions in member accounts will be able to see this trail \(including the trail ARN\) when they log into the AWS CloudTrail console from their AWS accounts, or when they run AWS CLI commands such as `describe-trails` \(although member accounts must use the ARN for the organization trail, and not the name, when using the AWS CLI\)\. However, users in member accounts will not have sufficient permissions to delete the organization trail, turn logging on or off, change what types of events are logged, or otherwise alter the organization trail in any way\. For more information about AWS Organizations, see [Organizations Terminology and Concepts](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html)\. For more information about creating and working with organization trails, see [Creating a trail for an organization](creating-trail-organization.md)\.

## How do you manage CloudTrail?<a name="cloudtrail-concepts-manage"></a>

### CloudTrail console<a name="cloudtrail-concepts-console"></a>

You can use and manage the CloudTrail service with the AWS CloudTrail console\. The console provides a user interface for performing many CloudTrail tasks such as:
+ Viewing recent events and event history for your AWS account\.
+ Downloading a filtered or complete file of the last 90 days of events\.
+ Creating and editing CloudTrail trails\.
+ Configuring CloudTrail trails, including: 
  + Selecting an Amazon S3 bucket\.
  + Setting a prefix\.
  + Configuring delivery to CloudWatch Logs\.
  + Using AWS KMS keys for encryption\. 
  + Enabling Amazon SNS notifications for log file delivery\.
  + Adding and managing tags for your trails\.

Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.

For more information about the AWS Management Console, see [AWS Management Console](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html)\.

### CloudTrail CLI<a name="cloudtrail-concepts-cli"></a>

The AWS Command Line Interface is a unified tool that you can use to interact with CloudTrail from the command line\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For a complete list of CloudTrail CLI commands, see [Available Commands](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/index.html)\.

### CloudTrail APIs<a name="cloudtrail-concepts-api"></a>

In addition to the console and the CLI, you can also use the CloudTrail RESTful APIs to program CloudTrail directly\. For more information, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

### AWS SDKs<a name="cloudtrail-concepts-sdk"></a>

As an alternative to using the CloudTrail API, you can use one of the AWS SDKs\. Each SDK consists of libraries and sample code for various programming languages and platforms\. The SDKs provide a convenient way to create programmatic access to CloudTrail\. For example, you can use the SDKs to sign requests cryptographically, manage errors, and retry requests automatically\. For more information, see the [Tools for Amazon Web Services](https://aws.amazon.com/tools/) page\.

### Why use tags for trails?<a name="cloudtrail-concepts-tags"></a>

A tag is a customer\-defined key and optional value that can be assigned to AWS resources, such as CloudTrail trails, Amazon S3 buckets used to store CloudTrail log files, AWS Organizations organizations and organizational units, and many more\. By adding the same tags to trails and to the Amazon S3 buckets you use to store log files for trails, you can make it easier to manage, search for, and filter these resources with [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/)\. You can implement tagging strategies to help you consistently, effectively, and easily find and manage your resources\. For more information, see [AWS Tagging Strategies](https://aws.amazon.com/answers/account-management/aws-tagging-strategies/)\.

## How do you control access to CloudTrail?<a name="cloudtrail-concepts-iam"></a>

AWS Identity and Access Management is a web service that enables Amazon Web Services \(AWS\) customers to manage users and user permissions\.  Use IAM to create individual users for anyone who needs access to AWS CloudTrail\. Create an IAM user for yourself, give that IAM user administrative privileges, and use that IAM user for all of your work\. By creating individual IAM users for people accessing your account, you can give each IAM user a unique set of security credentials\. You can also grant different permissions to each IAM user\. If necessary, you can change or revoke an IAM user’s permissions at any time\. For more information, see [Controlling User Permissions for CloudTrail](control-user-permissions-for-cloudtrail.md)\.

## How do you log management and data events?<a name="understanding-event-selectors"></a>

By default, trails log all management events for your AWS account and don't include data events\. You can choose to create or update trails to log data events\. Only events that match your trail settings are delivered to your Amazon S3 bucket, and optionally to an Amazon CloudWatch Logs log group\. If the event doesn't match the settings for a trail, the trail doesn't log the event\. For more information, see [Working with CloudTrail Log Files](cloudtrail-working-with-log-files.md)\. 

## How do you log CloudTrail Insights events?<a name="understanding-insight-selectors"></a>

AWS CloudTrail Insights helps AWS users identify and respond to unusual volumes of API calls by continuously analyzing CloudTrail management events\. An Insights event is a record of unusual levels of `write` management API activity\. The details page of an Insights event shows the event as a graph of unusual activity, and shows the start and end times of the unusual activity, along with the baseline that is used to determine whether the activity is unusual\. By default, trails don't log CloudTrail Insights events\. In the console, you can choose to log Insights events when you create or update a trail\. When you use the CloudTrail API, you can log Insights events by editing the settings of an existing trail with the PutInsightSelectors API\. Additional charges apply for logging CloudTrail Insights events\. For more information, see [Logging Insights Events for Trails](logging-insights-events-with-cloudtrail.md) and [AWS CloudTrail Pricing](http://aws.amazon.com/cloudtrail/pricing/)\.

## How do you perform monitoring with CloudTrail?<a name="cloudtrail-concepts-monitoring"></a>

### CloudWatch Logs, CloudWatch Events, and CloudTrail<a name="cloudtrail-concepts-cloudwatch-logs"></a>

Amazon CloudWatch is a web service that collects and tracks metrics to monitor your Amazon Web Services \(AWS\) resources and the applications that you run on AWS\. Amazon CloudWatch Logs is a feature of CloudWatch that you can use specifically to monitor log data\. Integration with CloudWatch Logs enables CloudTrail to send events containing API activity in your AWS account to a CloudWatch Logs log group\. CloudTrail events that are sent to CloudWatch Logs can trigger alarms according to the metric filters you define\. You can optionally configure CloudWatch alarms to send notifications or make changes to the resources that you are monitoring based on log stream events that your metric filters extract\. Using CloudWatch Logs, you can also track CloudTrail events alongside events from the operating system, applications, or other AWS services that are sent to CloudWatch Logs\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

Amazon CloudWatch Events is an AWS service that delivers a near real\-time stream of system events that describe changes in AWS resources\. In CloudWatch Events, you can create rules that trigger on any event recorded by CloudTrail\. For more information, see [Creating a CloudWatch Events Rule That Triggers on an AWS API Call Using AWS CloudTrail](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-CloudTrail-Rule.html)\.

Insights events are integrated with CloudWatch\. You can deliver events that you are subscribed to on your trail, including Insights events, to CloudWatch Events and CloudWatch Logs\. To configure CloudWatch Events with the CloudWatch console or API, choose the `AWS Insight via CloudTrail` event type on the **Create rule** page in the CloudWatch console\.

Sending data that is logged by CloudTrail to CloudWatch Logs or CloudWatch Events requires that you have at least one trail\. For more information about how to create a trail, see [Creating a Trail](cloudtrail-create-a-trail-using-the-console-first-time.md)\.

## How does CloudTrail behave regionally and globally?<a name="cloudtrail-concepts-regional-and-global-services"></a>

 A trail can be applied to all Regions or a single Region\. As a best practice, create a trail that applies to all Regions in the [AWS partition](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in which you are working\. This is the default setting when you create a trail in the CloudTrail console\.

**Note**  
*Turning on a trail* means that you create a trail and start delivery of CloudTrail event log files to an Amazon S3 bucket\. In the CloudTrail console, logging is turned on automatically when you create a trail\.

### What are the advantages of applying a trail to all Regions?<a name="cloudtrail-concepts-trails-enable-all-regions-advantages"></a>

A trail that applies to all AWS Regions has the following advantages:
+ The configuration settings for the trail apply consistently across all AWS Regions\.
+ You receive CloudTrail events from all AWS Regions in a single Amazon S3 bucket and, optionally, in a CloudWatch Logs log group\.
+ You manage trail configuration for all AWS Regions from one location\. 
+ You immediately receive events from a new AWS Region\. When a new AWS Region is launched, CloudTrail automatically creates a copy of all of your Region trails for you in the new Region with the same settings as your original trail\.
+ You don't need to create trails in AWS Regions that you don't use often in order to monitor for unusual activity\. Any activity in any AWS Region is logged in a trail that applies to all AWS Regions\.

### What happens when you apply a trail to all Regions?<a name="cloudtrail-concepts-trails-enable-all-regions"></a>

 When you apply a trail to all AWS Regions, CloudTrail uses the trail that you create in a particular Region to create trails with identical configurations in all other Regions in your account\.

This has the following effects:
+ CloudTrail delivers log files for account activity from all AWS Regions to the single Amazon S3 bucket that you specify, and, optionally, to a CloudWatch Logs log group\.
+ If you configured an Amazon SNS topic for the trail, SNS notifications about log file deliveries in all AWS Regions are sent to that single SNS topic\.
+ If you enabled it, log file integrity validation is enabled for the trail in all AWS Regions\. For information, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.

### Multiple trails per Region<a name="cloudtrail-concepts-trails-multiple-trails-per-region"></a>

 If you have different but related user groups, such as developers, security personnel, and IT auditors, you can create multiple trails per Region\. This allows each group to receive its own copy of the log files\. 

 CloudTrail supports five trails per Region\. A trail that applies to all AWS Regions counts as one trail in every Region\. 

The following example is a Region with five trails:
+ You create two trails in the US West \(N\. California\) Region that apply to this Region only\.
+ You create two more trails in US West \(N\. California\) Region that apply to all AWS Regions\. 
+ You create a trail in the Asia Pacific \(Sydney\) Region that applies to all AWS Regions\. This trail also exists as a trail in the US West \(N\. California\) Region\.

Trails appear in the AWS Region where they exist\. Trails that log events in all AWS Regions appear in every Region\. You can view a list of trails in an AWS Region in the **Trails** page of the CloudTrail console\. For more information, see [Updating a trail](cloudtrail-update-a-trail-console.md)\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

### AWS Security Token Service \(AWS STS\) and CloudTrail<a name="cloudtrail-concepts-sts-regionalization"></a>

AWS STS is a service that has a global endpoint and also supports region\-specific endpoints\. An endpoint is a URL that is the entry point for web service requests\. For example, `https://cloudtrail.us-west-2.amazonaws.com` is the US West \(Oregon\) regional entry point for the AWS CloudTrail service\. Regional endpoints help reduce latency in your applications\. 

When you use an AWS STS region\-specific endpoint, the trail in that region delivers only the AWS STS events that occur in that region\. For example, if you are using the endpoint `sts.us-west-2.amazonaws.com`, the trail in us\-west\-2 delivers only the AWS STS events that originate from us\-west\-2\. For more information about AWS STS regional endpoints, see [Activating and Deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

For a complete list of AWS regional endpoints, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *AWS General Reference*\. For details about events from the global AWS STS endpoint, see [About global service events](#cloudtrail-concepts-global-service-events)\.

## About global service events<a name="cloudtrail-concepts-global-service-events"></a>

For most services, events are recorded in the region where the action occurred\. For global services such as AWS Identity and Access Management \(IAM\), AWS STS, and Amazon CloudFront, events are delivered to any trail that includes global services\.

For most global services, events are logged as occurring in US East \(N\. Virginia\) Region, but some global service events are logged as occurring in other regions, such as US East \(Ohio\) Region or US West \(Oregon\) Region\.

To avoid receiving duplicate global service events, remember the following:
+ Global service events are delivered by default to trails that are created using the CloudTrail console\. Events are delivered to the bucket for the trail\.
+ If you have multiple single region trails, consider configuring your trails so that global service events are delivered in only one of the trails\. For more information, see [Enabling and disabling logging global service events](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses)\. 
+ If you change the configuration of a trail from logging all regions to logging a single region, global service event logging is turned off automatically for that trail\. Similarly, if you change the configuration of a trail from logging a single region to logging all regions, global service event logging is turned on automatically for that trail\. 

  For more information about changing global service event logging for a trail, see [Enabling and disabling logging global service events](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses)\.

**Example:**

1. You create a trail in the CloudTrail console\. By default, this trail logs global service events\.

1. You have multiple single region trails\.

1. You do not need to include global services for the single region trails\. Global service events are delivered for the first trail\. For more information, see [Creating, updating, and managing trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

**Note**  
 When you create or update a trail with the AWS CLI, AWS SDKs, or CloudTrail API, you can specify whether to include or exclude global service events for trails\. You cannot configure global service event logging from the CloudTrail console\.

## How does CloudTrail relate to other AWS monitoring services?<a name="cloudtrail-concepts-other-aws-monitoring-services"></a>

CloudTrail adds another dimension to the monitoring capabilities already offered by AWS\. It does not change or replace logging features you might already be using, such as those for Amazon S3 or Amazon CloudFront subscriptions\. Amazon CloudWatch focuses on performance monitoring and system health\. CloudTrail focuses on API activity\. Although CloudTrail does not report on system performance or health, you can use CloudTrail with CloudWatch alarms to notify you about activity that you might be interested in\. 

## Partner solutions<a name="cloudtrail-concepts-partner-solutions"></a>

AWS partners with third\-party specialists in logging and analysis to provide solutions that use CloudTrail output\. For more information, visit the CloudTrail detail page at [AWS CloudTrail](https://aws.amazon.com/cloudtrail)\.