# Monitoring CloudTrail Log Files with Amazon CloudWatch Logs<a name="monitor-cloudtrail-log-files-with-cloudwatch-logs"></a>

You can configure CloudTrail with CloudWatch Logs to monitor your trail logs and be notified when specific activity occurs\. 

1. Configure your trail to send log events to CloudWatch Logs\.

1. Define CloudWatch Logs metric filters to evaluate log events for matches in terms, phrases, or values\. For example, you can monitor for `ConsoleLogin` events\. 

1. Assign CloudWatch metrics to the metric filters\.

1. Create CloudWatch alarms that are triggered according to thresholds and time periods that you specify\. You can configure alarms to send notifications when alarms are triggered, so that you can take action\.

1. You can also configure CloudWatch to automatically perform an action in response to an alarm\. 

Standard pricing for Amazon CloudWatch and Amazon CloudWatch Logs applies\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

You can configure your trails to send logs to CloudWatch Logs in the following regions:


****  

| Region Name | Region | 
| --- | --- | 
| US East \(Ohio\) | us\-east\-2 | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| US West \(N\. California\) | us\-west\-1 | 
| US West \(Oregon\) | us\-west\-2 | 
| Canada \(Central\) | ca\-central\-1 | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | 
| EU \(Frankfurt\) | eu\-central\-1 | 
| EU \(Ireland\) | eu\-west\-1 | 
| EU \(London\) | eu\-west\-2 | 
| South America \(São Paulo\) | sa\-east\-1 | 
| AWS GovCloud \(US\)\* | us\-gov\-west\-1 | 

\* This region requires a separate account\. For more information, see [AWS GovCloud \(US\)](https://aws.amazon.com/govcloud-us/)\.


+ [Sending Events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)
+ [Creating CloudWatch Alarms with an AWS CloudFormation Template](use-cloudformation-template-to-create-cloudwatch-alarms.md)
+ [Creating CloudWatch Alarms for CloudTrail Events: Examples](cloudwatch-alarms-for-cloudtrail.md)
+ [Creating CloudWatch Alarms for CloudTrail Events: Additional Examples](cloudwatch-alarms-for-cloudtrail-additional-examples.md)
+ [Configuring Notifications for CloudWatch Logs Alarms](cloudtrail-configure-notifications-for-cloudwatch-logs-alarms.md)
+ [Stopping CloudTrail from Sending Events to CloudWatch Logs](stop-cloudtrail-from-sending-events-to-cloudwatch-logs.md)
+ [CloudWatch Log Group and Log Stream Naming for CloudTrail](cloudwatch-log-group-log-stream-naming-for-cloudtrail.md)
+ [Role Policy Document for CloudTrail to Use CloudWatch Logs for Monitoring](cloudtrail-required-policy-for-cloudwatch-logs.md)