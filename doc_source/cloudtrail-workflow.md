# CloudTrail workflow<a name="cloudtrail-workflow"></a>

**View event history for your AWS account**  
You can view and search the last 90 days of events recorded by CloudTrail in the CloudTrail console or by using the AWS CLI\. For more information, see [Viewing events with CloudTrail Event history](view-cloudtrail-events.md)\.

**Download events**  
You can download a CSV or JSON file containing up to the past 90 days of CloudTrail events for your AWS account\. For more information, see [Downloading events](view-cloudtrail-events-console.md#downloading-events) or [Downloading Insights events](view-insights-events-console.md#downloading-insights-events)\.

**Enable CloudTrail Lake**  
CloudTrail Lake lets you run fine\-grained SQL\-based queries on events from both AWS sources, and sources outside of AWS\. Events are aggregated into event data stores, which are immutable collections of events based on criteria that you select by applying [advanced event selectors](logging-data-events-with-cloudtrail.md#creating-data-event-selectors-advanced)\. You can keep the event data in an event data store for up to seven years\. CloudTrail Lake is part of an auditing solution that helps you perform security investigations and troubleshooting\. For more information, see [Working with AWS CloudTrail Lake](cloudtrail-lake.md)\.

**Copy trail events to CloudTrail Lake**  
You can copy existing trail events to a CloudTrail Lake event data store to create a point\-in\-time snapshot of events logged to the trail\. For more information, see [Copying trail events to CloudTrail Lake](cloudtrail-copy-trail-to-lake.md)\.

**Save CloudTrail Lake query results to an Amazon S3 bucket**  
When you run a query, you can save the query results to an S3 bucket\. For more information, see [Run a query and save query results](query-run-query.md)\.

**Download saved query results**  
You can download a CSV file containing your saved CloudTrail Lake query results\. For more information, see [Download your CloudTrail Lake saved query results](cloudtrail-download-lake-query-results.md)\.

** Create a trail **  
A trail enables CloudTrail to deliver log files to your Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions\. The trail logs events from all regions in the AWS partition and delivers the log files to the S3 bucket that you specify\. For more information, see [Creating a trail for your AWS account](cloudtrail-create-and-update-a-trail.md)\.

** Create and subscribe to an Amazon SNS topic**  
Subscribe to a topic to receive notifications about log file delivery to your bucket\. Amazon SNS can notify you in multiple ways, including programmatically with Amazon Simple Queue Service\. For information, see [Configuring Amazon SNS notifications for CloudTrail](configure-sns-notifications-for-cloudtrail.md)\.  
If you want to receive SNS notifications about log file deliveries from all regions, specify only one SNS topic for your trail\. If you want to programmatically process all events, see [Using the CloudTrail Processing Library](use-the-cloudtrail-processing-library.md)\.

** View your log files **  
Use Amazon S3 to retrieve log files\. For information, see [Getting and viewing your CloudTrail log files](get-and-view-cloudtrail-log-files.md)\.

** Manage user permissions **  
Use AWS Identity and Access Management \(IAM\) to manage which users have permissions to create, configure, or delete trails; start and stop logging; and access buckets that have log files\. For more information, see [Controlling user permissions for CloudTrail](control-user-permissions-for-cloudtrail.md)\.

** Monitor events with CloudWatch Logs **  
You can configure your trail to send events to CloudWatch Logs\. You can then use CloudWatch Logs to monitor your account for specific API calls and events\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.  
If you configure a trail that applies to all regions to send events to a CloudWatch Logs log group, CloudTrail sends events from all regions to a single log group\.

** Log management and data events **  
Configure your trails to log read\-only, write\-only, or all management and data events\. By default, trails log management events\. For more information, see [Working with CloudTrail log files](cloudtrail-working-with-log-files.md)\.

**Log CloudTrail Insights events**  
Configure your trails to log Insights events to help you identify and respond to unusual activity associated with `write` management API calls\. If your trail is configured to log read\-only or no management events, you cannot turn on CloudTrail Insights event logging\. For more information, see [Logging Insights events for trails](logging-insights-events-with-cloudtrail.md)\.

** Enable log encryption **  
Log file encryption provides an extra layer of security for your log files\. For more information, see [Encrypting CloudTrail log files with AWS KMS keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)\.

** Enable log file integrity **  
Log file integrity validation helps you verify that log files have remained unchanged since CloudTrail delivered them\. For more information, see [Validating CloudTrail log file integrity](cloudtrail-log-file-validation-intro.md)\.

**Share log files with other AWS accounts**  
You can share log files between accounts\. For more information, see [Sharing CloudTrail log files between AWS accounts](cloudtrail-sharing-logs.md)\.

** Aggregate logs from multiple accounts **  
You can aggregate log files from multiple accounts to a single bucket\. For more information, see [Receiving CloudTrail log files from multiple accountsRedacting bucket owner account IDs for data events called by other accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.

** Register a delegated administrator to manage your organization's CloudTrail resources **  
You can register a delegated administrator to manage your organization's CloudTrail trails and event data stores\. For more information, see [Organization delegated administrator](cloudtrail-delegated-administrator.md)\.

** Work with partner solutions **  
Analyze your CloudTrail output with a partner solution that integrates with CloudTrail\. Partner solutions offer a broad set of capabilities, such as change tracking, troubleshooting, and security analysis\. For more information, see the [AWS CloudTrail](https://aws.amazon.com/cloudtrail/partners/) partner page\.