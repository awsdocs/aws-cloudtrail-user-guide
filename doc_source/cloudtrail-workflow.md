# CloudTrail Workflow<a name="cloudtrail-workflow"></a>

**View event history for your AWS account**  
You can view and search the last 90 days of events recorded by CloudTrail in the CloudTrail console or by using the AWS CLI\. For more information, see [Viewing Events with CloudTrail Event History](view-cloudtrail-events.md)\.

**Download events**  
You can download a CSV or JSON file containing up to the past 90 days of CloudTrail events for your AWS account\. For more information, see [Downloading Events](view-cloudtrail-events-console.md#downloading-events)\.

** Create a trail **  
A trail enables CloudTrail to deliver log files to your Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions\. The trail logs events from all regions in the AWS partition and delivers the log files to the S3 bucket that you specify\. For more information, see [Creating a Trail For Your AWS Account](cloudtrail-create-and-update-a-trail.md)\.

** Create and subscribe to an Amazon SNS topic**  
Subscribe to a topic to receive notifications about log file delivery to your bucket\. Amazon SNS can notify you in multiple ways, including programmatically with Amazon Simple Queue Service\. For information, see [Configuring Amazon SNS Notifications for CloudTrail](configure-sns-notifications-for-cloudtrail.md)\.  
If you want to receive SNS notifications about log file deliveries from all regions, specify only one SNS topic for your trail\. If you want to programmatically process all events, see [Using the CloudTrail Processing Library](use-the-cloudtrail-processing-library.md)\.

** View your log files **  
Use Amazon S3 to retrieve log files\. For information, see [Getting and Viewing Your CloudTrail Log Files](get-and-view-cloudtrail-log-files.md)\.

** Manage user permissions **  
Use AWS Identity and Access Management \(IAM\) to manage which users have permissions to create, configure, or delete trails; start and stop logging; and access buckets that have log files\. For more information, see [Controlling User Permissions for CloudTrail](control-user-permissions-for-cloudtrail.md)\.

** Monitor events with CloudWatch Logs **  
You can configure your trail to send events to CloudWatch Logs\. You can then use CloudWatch Logs to monitor your account for specific API calls and events\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.  
If you configure a trail that applies to all regions to send events to a CloudWatch Logs log group, CloudTrail sends events from all regions to a single log group\.

** Log management and data events **  
Configure your trails to log read\-only, write\-only, or all management and data events\. By default, trails log management events\. For more information, see [Logging Data and Management Events for Trails](logging-management-and-data-events-with-cloudtrail.md)\.

** Enable log encryption **  
Log file encryption provides an extra layer of security for your log files\. For more information, see [Encrypting CloudTrail Log Files with AWS KMS–Managed Keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)\.

** Enable log file integrity **  
Log file integrity validation helps you verify that log files have remained unchanged since CloudTrail delivered them\. For more information, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.

**Share log files with other AWS accounts**  
You can share log files between accounts\. For more information, see [Sharing CloudTrail Log Files Between AWS Accounts](cloudtrail-sharing-logs.md)\.

** Aggregate logs from multiple accounts **  
You can aggregate log files from multiple accounts to a single bucket\. For more information, see [Receiving CloudTrail Log Files from Multiple Accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.

** Work with partner solutions **  
Analyze your CloudTrail output with a partner solution that integrates with CloudTrail\. Partner solutions offer a broad set of capabilities, such as change tracking, troubleshooting, and security analysis\. For more information, see the [AWS CloudTrail](https://aws.amazon.com/cloudtrail/partners/) partner page\.