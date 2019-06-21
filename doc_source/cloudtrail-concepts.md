# CloudTrail Concepts<a name="cloudtrail-concepts"></a>

This section summarizes basic concepts related to CloudTrail\.

**Contents**
+ [What Are CloudTrail Events?](#cloudtrail-concepts-events)
  + [What Are Management Events?](#cloudtrail-concepts-management-events)
  + [What Are Data Events?](#cloudtrail-concepts-data-events)
+ [What Is CloudTrail Event History?](#cloudtrail-concepts-event-history)
+ [What Are Trails?](#cloudtrail-concepts-trails)
+ [What Are Organization Trails?](#cloudtrail-concepts-trails-org)
+ [How Do You Manage CloudTrail?](#cloudtrail-concepts-manage)
  + [CloudTrail Console](#cloudtrail-concepts-console)
  + [CloudTrail CLI](#cloudtrail-concepts-cli)
  + [CloudTrail APIs](#cloudtrail-concepts-api)
  + [AWS SDKs](#cloudtrail-concepts-sdk)
+ [How Do You Control Access to CloudTrail?](#cloudtrail-concepts-iam)
+ [How Do You Log Management and Data Events?](#understanding-event-selectors)
+ [How Do You Perform Monitoring with CloudTrail?](#cloudtrail-concepts-monitoring)
  + [CloudWatch Logs and CloudTrail](#cloudtrail-concepts-cloudwatch-logs)
+ [How Does CloudTrail Behave Regionally and Globally?](#cloudtrail-concepts-regional-and-global-services)
  + [What Are the Advantages of Applying a Trail to All Regions?](#cloudtrail-concepts-trails-enable-all-regions-advantages)
  + [What Happens When You Apply a Trail to All Regions?](#cloudtrail-concepts-trails-enable-all-regions)
  + [Multiple Trails per Region](#cloudtrail-concepts-trails-multiple-trails-per-region)
  + [AWS Security Token Service \(AWS STS\) and CloudTrail](#cloudtrail-concepts-sts-regionalization)
+ [About Global Service Events](#cloudtrail-concepts-global-service-events)
+ [How Does CloudTrail Relate to Other AWS Monitoring Services?](#cloudtrail-concepts-other-aws-monitoring-services)
+ [Partner Solutions](#cloudtrail-concepts-partner-solutions)

## What Are CloudTrail Events?<a name="cloudtrail-concepts-events"></a>

An event in CloudTrail is the record of an activity in an AWS account\. This activity can be an action taken by a user, role, or service that is monitorable by CloudTrail\. CloudTrail events provide a history of both API and non\-API account activity made through the AWS Management Console, AWS SDKs, command line tools, and other AWS services\. There are two types of events that can be logged in CloudTrail: management events and data events\. By default, trails log management events, but not data events\.

Both management events and data events use the same CloudTrail JSON log format\. You can identify them by the value in the `managementEvent` field\.

**Note**  
CloudTrail does not log all AWS services\. Some AWS services do not enable logging of all APIs and events\. Even if you configure logging all management and data events in a trail, you will not create a log with all possible AWS events\. For more information about unsupported services, see [CloudTrail Unsupported Services](cloudtrail-unsupported-aws-services.md)\. For details about which APIs are logged for a specific service, see documentation for that service in [CloudTrail Supported Services and Integrations](cloudtrail-aws-service-specific-topics.md)\.

### What Are Management Events?<a name="cloudtrail-concepts-management-events"></a>

Management events provide insight into management operations that are performed on resources in your AWS account\. These are also known as *control plane operations*\. Example management events include:
+ Configuring security \(for example, IAM `AttachRolePolicy` API operations\)\.
+ Registering devices \(for example, Amazon EC2 `CreateDefaultVpc` API operations\)\.
+ Configuring rules for routing data \(for example, Amazon EC2 `CreateSubnet` API operations\)\.
+ Setting up logging \(for example, AWS CloudTrail `CreateTrail` API operations\)\.

Management events can also include non\-API events that occur in your account\. For example, when a user signs in to your account, CloudTrail logs the `ConsoleLogin` event\. For more information, see [Non\-API Events Captured by CloudTrail](cloudtrail-non-api-events.md)\. For a list of management events that CloudTrail logs for AWS services, see [CloudTrail Supported Services and Integrations](cloudtrail-aws-service-specific-topics.md)\.

### What Are Data Events?<a name="cloudtrail-concepts-data-events"></a>

Data events provide insight into the resource operations performed on or in a resource\. These are also known as *data plane operations*\. Data events are often high\-volume activities\. Example data events include:
+  Amazon S3 object\-level API activity \(for example, `GetObject`, `DeleteObject`, and `PutObject` API operations\)\.
+ AWS Lambda function execution activity \(the `Invoke` API\)\.

 Data events are disabled by default when you create a trail\. To record CloudTrail data events, you must explicitly add to a trail the supported resources or resource types for which you want to collect activity\. For more information, see [Creating a Trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.

Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

## What Is CloudTrail Event History?<a name="cloudtrail-concepts-event-history"></a>

CloudTrail event history provides a viewable, searchable, and downloadable record of the past 90 days of CloudTrail events\. You can use this history to gain visibility into actions taken in your AWS account in the AWS Management Console, AWS SDKs, command line tools, and other AWS services\. You can customize your view of event history in the CloudTrail console by selecting which columns are displayed\. 

## What Are Trails?<a name="cloudtrail-concepts-trails"></a>

 A trail is a configuration that enables delivery of CloudTrail events to an Amazon S3 bucket, CloudWatch Logs, and CloudWatch Events\. You can use a trail to filter the CloudTrail events you want delivered, encrypt your CloudTrail event log files with an AWS KMS key, and set up Amazon SNS notifications for log file delivery\. For more information about how to create and manage a trail, see [Creating a Trail For Your AWS Account](cloudtrail-create-and-update-a-trail.md)\.

## What Are Organization Trails?<a name="cloudtrail-concepts-trails-org"></a>

 An organization trail is a configuration that enables delivery of CloudTrail events in the master account and all member accounts in an organization to the same Amazon S3 bucket, CloudWatch Logs, and CloudWatch Events\. Creating an organization trail helps you define a uniform event logging strategy for your organization\. 

When you create an organization trail, a trail with the name that you give it will be created in every AWS account that belongs to your organization\. Users with CloudTrail permissions in member accounts will be able to see this trail \(including the trail ARN\) when they log into the AWS CloudTrail console from their AWS accounts, or when they run AWS CLI commands such as `describe-trails` \(although member accounts must use the ARN for the organization trail, and not the name, when using the AWS CLI\)\. However, users in member accounts will not have sufficient permissions to delete the organization trail, turn logging on or off, change what types of events are logged, or otherwise alter the organization trail in any way\. For more information about AWS Organizations, see [Organizations Terminology and Concepts](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html)\. For more information about creating and working with organization trails, see [Creating a Trail for an Organization](creating-trail-organization.md)\.

## How Do You Manage CloudTrail?<a name="cloudtrail-concepts-manage"></a>

### CloudTrail Console<a name="cloudtrail-concepts-console"></a>

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

Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.

For more information about the AWS Management Console, see [AWS Management Console](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html)\. 

### CloudTrail CLI<a name="cloudtrail-concepts-cli"></a>

The AWS Command Line Interface is a unified tool that you can use to interact with CloudTrail from the command line\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For a complete list of CloudTrail CLI commands, see [Available Commands](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/index.html)\.

### CloudTrail APIs<a name="cloudtrail-concepts-api"></a>

In addition to the console and the CLI, you can also use the CloudTrail RESTful APIs to program CloudTrail directly\. For more information, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

### AWS SDKs<a name="cloudtrail-concepts-sdk"></a>

As an alternative to using the CloudTrail API, you can use one of the AWS SDKs\. Each SDK consists of libraries and sample code for various programming languages and platforms\. The SDKs provide a convenient way to create programmatic access to CloudTrail\. For example, you can use the SDKs to sign requests cryptographically, manage errors, and retry requests automatically\. For more information, see the [Tools for Amazon Web Services](https://aws.amazon.com/tools/) page\.

## How Do You Control Access to CloudTrail?<a name="cloudtrail-concepts-iam"></a>

AWS Identity and Access Management is a web service that enables Amazon Web Services \(AWS\) customers to manage users and user permissions\.  Use IAM to create individual users for anyone who needs access to AWS CloudTrail\. Create an IAM user for yourself, give that IAM user administrative privileges, and use that IAM user for all of your work\. By creating individual IAM users for people accessing your account, you can give each IAM user a unique set of security credentials\. You can also grant different permissions to each IAM user\. If necessary, you can change or revoke an IAM user’s permissions at any time\. For more information, see [Controlling User Permissions for CloudTrail](control-user-permissions-for-cloudtrail.md)\.

## How Do You Log Management and Data Events?<a name="understanding-event-selectors"></a>

By default, trails log all management events for your AWS account and don't include data events\. You can choose to create or update trails to log data events\. Only events that match your trail settings are delivered to your Amazon S3 bucket and Amazon CloudWatch Events, and optionally to an Amazon CloudWatch Logs log group\. If the event doesn't match the settings for a trail, the trail doesn't log the event\. For more information, see [Logging Data and Management Events for Trails](logging-management-and-data-events-with-cloudtrail.md)\. 

## How Do You Perform Monitoring with CloudTrail?<a name="cloudtrail-concepts-monitoring"></a>

### CloudWatch Logs and CloudTrail<a name="cloudtrail-concepts-cloudwatch-logs"></a>

Amazon CloudWatch is a web service that collects and tracks metrics to monitor your Amazon Web Services \(AWS\) resources and the applications that you run on AWS\. Amazon CloudWatch Logs is a feature of CloudWatch that you can use specifically to monitor log data\. Integration with CloudWatch Logs enables CloudTrail to send events containing API activity in your AWS account to a CloudWatch Logs log group\. CloudTrail events that are sent to CloudWatch Logs can trigger alarms according to the metric filters you define\. You can optionally configure CloudWatch alarms to send notifications or make changes to the resources that you are monitoring based on log stream events that your metric filters extract\. Using CloudWatch Logs, you can also track CloudTrail events alongside events from the operating system, applications, or other AWS services that are sent to CloudWatch Logs\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

## How Does CloudTrail Behave Regionally and Globally?<a name="cloudtrail-concepts-regional-and-global-services"></a>

 A trail can be applied to all regions or a single region\. As a best practice, create a trail that applies to all regions in the [AWS partition](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in which you are working\. This is the default setting when you create a trail in the CloudTrail console\.

**Note**  
*Turning on a trail* means that you create a trail and start delivery of CloudTrail event log files to an Amazon S3 bucket\. In the CloudTrail console, logging is turned on automatically when you create a trail\.

### What Are the Advantages of Applying a Trail to All Regions?<a name="cloudtrail-concepts-trails-enable-all-regions-advantages"></a>

A trail that applies to all regions has the following advantages:
+ The configuration settings for the trail apply consistently across all regions\.
+ You receive CloudTrail events from all regions in a single S3 bucket and, optionally, in a CloudWatch Logs log group\.
+ You manage trail configuration for all regions from one location\. 
+ You immediately receive events from a new region\. When a new region is launched, CloudTrail automatically creates a trail for you in the new region with the same settings as your original trail\.
+ You can create trails in regions that you don't use often to monitor for unusual activity\.

### What Happens When You Apply a Trail to All Regions?<a name="cloudtrail-concepts-trails-enable-all-regions"></a>

 When you apply a trail to all regions, CloudTrail uses the trail that you create in a particular region to create trails with identical configurations in all other regions in your account\.

 This has the following effects:
+ CloudTrail delivers log files for account activity from all regions to the single Amazon S3 bucket that you specify, and, optionally, to a CloudWatch Logs log group\.
+ If you configured an Amazon SNS topic for the trail, SNS notifications about log file deliveries in all regions are sent to that single SNS topic\.
+ If you enabled it, log file integrity validation is enabled for the trail in all regions\. For information, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.

### Multiple Trails per Region<a name="cloudtrail-concepts-trails-multiple-trails-per-region"></a>

 If you have different but related user groups, such as developers, security personnel, and IT auditors, you can create multiple trails per region\. This allows each group to receive its own copy of the log files\. 

 CloudTrail supports five trails per region\. A trail that applies to all regions counts as one trail in every region\. 

The following example is a region with five trails:
+ You create two trails in the US West \(N\. California\) Region that apply to this region only\.
+ You create two more trails in US West \(N\. California\) Region that apply to all regions\. 
+ You create a trail in the Asia Pacific \(Sydney\) Region that applies to all regions\. This trail also exists as a trail in the US West \(N\. California\) Region\.

You can see a list of your trails in all regions on the **Trails** page of the CloudTrail console\. For more information, see [Updating a Trail](cloudtrail-update-a-trail-console.md)\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

### AWS Security Token Service \(AWS STS\) and CloudTrail<a name="cloudtrail-concepts-sts-regionalization"></a>

AWS STS is a service that has a global endpoint and also supports region\-specific endpoints\. An endpoint is a URL that is the entry point for web service requests\. For example, `https://cloudtrail.us-west-2.amazonaws.com` is the US West \(Oregon\) regional entry point for the AWS CloudTrail service\. Regional endpoints help reduce latency in your applications\. 

When you use an AWS STS region\-specific endpoint, the trail in that region delivers only the AWS STS events that occur in that region\. For example, if you are using the endpoint `sts.us-west-2.amazonaws.com`, the trail in us\-west\-2 delivers only the AWS STS events that originate from us\-west\-2\. For more information about AWS STS regional endpoints, see [Activating and Deactivating AWS STS in an AWS Region](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html) in the *IAM User Guide*\.

For a complete list of AWS regional endpoints, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *AWS General Reference*\. For details about events from the global AWS STS endpoint, see [About Global Service Events](#cloudtrail-concepts-global-service-events)\.

## About Global Service Events<a name="cloudtrail-concepts-global-service-events"></a>

For most services, events are recorded in the region where the action occurred\. For global services such as AWS Identity and Access Management \(IAM\), AWS STS, Amazon CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East \(N\. Virginia\) Region\. 

To avoid receiving duplicate global service events, remember the following:
+ Global service events are delivered by default to trails that are created using the CloudTrail console\. Events are delivered to the bucket for the trail\.
+ If you have multiple single region trails, consider configuring your trails so that global service events are delivered in only one of the trails\. For more information, see [Enabling and disabling logging global service events](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses)\. 
+ If you change the configuration of a trail from logging all regions to logging a single region, global service event logging is turned off automatically for that trail\. Similarly, if you change the configuration of a trail from logging a single region to logging all regions, global service event logging is turned on automatically for that trail\. 

  For more information about changing global service event logging for a trail, see [Enabling and disabling logging global service events](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses)\.

**Example:**

1. You create a trail in the CloudTrail console\. By default, this trail logs global service events\.

1. You have multiple single region trails\.

1. You do not need to include global services for the single region trails\. Global service events are delivered for the first trail\. For more information, see [Creating and Updating a Trail with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

**Note**  
 When you create or update a trail with the AWS CLI, AWS SDKs, or CloudTrail API, you can specify whether to include or exclude global service events for trails\. You cannot configure global service event logging from the CloudTrail console\.

## How Does CloudTrail Relate to Other AWS Monitoring Services?<a name="cloudtrail-concepts-other-aws-monitoring-services"></a>

CloudTrail adds another dimension to the monitoring capabilities already offered by AWS\. It does not change or replace logging features you might already be using, such as those for Amazon S3 or Amazon CloudFront subscriptions\. Amazon CloudWatch focuses on performance monitoring and system health\. CloudTrail focuses on API activity\. Although CloudTrail does not report on system performance or health, you can use CloudTrail with CloudWatch alarms to notify you about activity that you might be interested in\. 

## Partner Solutions<a name="cloudtrail-concepts-partner-solutions"></a>

AWS partners with third\-party specialists in logging and analysis to provide solutions that use CloudTrail output\. For more information, visit the CloudTrail detail page at [AWS CloudTrail](https://aws.amazon.com/cloudtrail)\.  