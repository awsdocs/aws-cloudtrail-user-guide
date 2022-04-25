# Working with CloudTrail log files<a name="cloudtrail-working-with-log-files"></a>

You can perform more advanced tasks with your CloudTrail files\.
+ Create multiple trails per region\.
+ Monitor CloudTrail log files by sending them to CloudWatch Logs\.
+ Share log files between accounts\. 
+ Use the AWS CloudTrail Processing Library to write log processing applications in Java\.
+ Validate your log files to verify that they have not changed after delivery by CloudTrail\.

When an event occurs in your account, CloudTrail evaluates whether the event matches the settings for your trails\. Only events that match your trail settings are delivered to your Amazon S3 bucket and Amazon CloudWatch Logs log group\.

You can configure multiple trails differently so that the trails process and log only the events that you specify\. For example, one trail can log read\-only data and management events, so that all read\-only events are delivered to one S3 bucket\. Another trail can log only write\-only data and management events, so that all write\-only events are delivered to a separate S3 bucket\. 

You can also configure your trails to have one trail log and deliver all management events to one S3 bucket, and configure another trail to log and deliver all data events to another S3 bucket\. 

You can configure your trails to log the following:
+ **[Data events](logging-data-events-with-cloudtrail.md)**: These events provide visibility into the resource operations performed on or within a resource\. These are also known as data plane operations\. 
+ **[Management events](logging-management-events-with-cloudtrail.md)**: Management events provide visibility into management operations that are performed on resources in your AWS account\. These are also known as control plane operations\. Management events can also include non\-API events that occur in your account\. For example, when a user logs in to your account, CloudTrail logs the `ConsoleLogin` event\. For more information, see [Non\-API events captured by CloudTrail](cloudtrail-non-api-events.md)\. 
**Note**  
Not all AWS services support CloudTrail events\. For more information about supported services, see [CloudTrail supported services and integrations](cloudtrail-aws-service-specific-topics.md)\. For specific details about what APIs are logged for a specific service, see that service's documentation in [CloudTrail supported services and integrations](cloudtrail-aws-service-specific-topics.md)\.
+ **[Insights events](logging-insights-events-with-cloudtrail.md)**: Insights events capture unusual activity that is detected in your account\. If you have Insights events enabled, and CloudTrail detects unusual activity, Insights events are logged to the destination S3 bucket for your trail, but in a different folder\. You can also see the type of Insights event and the incident time period when you view Insights events on the CloudTrail console\. Unlike other types of events captured in a CloudTrail trail, Insights events are logged only when CloudTrail detects changes in your account's API usage that differ significantly from the account's typical usage patterns\. 

  Insights events are generated only for **write** management APIs\.

**Topics**
+ [Create multiple trails](create-multiple-trails.md)
+ [Logging management events for trails](logging-management-events-with-cloudtrail.md)
+ [Logging data events for trails](logging-data-events-with-cloudtrail.md)
+ [Logging Insights events for trails](logging-insights-events-with-cloudtrail.md)
+ [Receiving CloudTrail log files from multiple regions](receive-cloudtrail-log-files-from-multiple-regions.md)
+ [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)
+ [Receiving CloudTrail log files from multiple accounts](cloudtrail-receive-logs-from-multiple-accounts.md)
+ [Sharing CloudTrail log files between AWS accounts](cloudtrail-sharing-logs.md)
+ [Validating CloudTrail log file integrity](cloudtrail-log-file-validation-intro.md)
+ [Using the CloudTrail Processing Library](use-the-cloudtrail-processing-library.md)