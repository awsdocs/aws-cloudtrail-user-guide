# Monitoring CloudTrail Log Files with Amazon CloudWatch Logs<a name="monitor-cloudtrail-log-files-with-cloudwatch-logs"></a>

You can configure CloudTrail with CloudWatch Logs to monitor your trail logs and be notified when specific activity occurs\. 

1. Configure your trail to send log events to CloudWatch Logs\.

1. Define CloudWatch Logs metric filters to evaluate log events for matches in terms, phrases, or values\. For example, you can monitor for `ConsoleLogin` events\. 

1. Assign CloudWatch metrics to the metric filters\.

1. Create CloudWatch alarms that are triggered according to thresholds and time periods that you specify\. You can configure alarms to send notifications when alarms are triggered, so that you can take action\.

1. You can also configure CloudWatch to automatically perform an action in response to an alarm\. 

Standard pricing for Amazon CloudWatch and Amazon CloudWatch Logs applies\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

For more information about the Regions in which you can configure your trails to send logs to CloudWatch Logs, see [Amazon CloudWatch Logs Regions and Quotas](https://docs.aws.amazon.com/general/latest/gr/cwl_region.html) in the *AWS General Reference*\.

The AWS GovCloud \(US\-West\) Region requires a separate account\. For more information, see [AWS GovCloud \(US\-West\)](https://aws.amazon.com/govcloud-us/)\.

**Topics**
+ [Sending events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)
+ [Creating CloudWatch alarms with an AWS CloudFormation template](use-cloudformation-template-to-create-cloudwatch-alarms.md)
+ [Creating CloudWatch alarms for CloudTrail events: examples](cloudwatch-alarms-for-cloudtrail.md)
+ [Stopping CloudTrail from sending events to CloudWatch Logs](stop-cloudtrail-from-sending-events-to-cloudwatch-logs.md)
+ [CloudWatch log group and log stream naming for CloudTrail](cloudwatch-log-group-log-stream-naming-for-cloudtrail.md)
+ [Role policy document for CloudTrail to use CloudWatch Logs for monitoring](cloudtrail-required-policy-for-cloudwatch-logs.md)