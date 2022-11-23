# Supported CloudWatch metrics<a name="cloudtrail-lake-cloudwatch-metrics"></a>

CloudTrail Lake supports Amazon CloudWatch metrics\. CloudWatch is a monitoring service for AWS resources\. You can use CloudWatch to collect and track metrics, set alarms, and automatically react to changes in your AWS resources\. 

 The `AWS/CloudTrail` namespace includes the following metrics for CloudTrail Lake\. 


****  

| Metric | Description | Units | 
| --- | --- | --- | 
| HourlyDataIngested | The amount of data ingested into the event data store during the last hour\. This metric is updated every hour\. | Bytes | 
| TotalDataRetained | The amount of data retained in the event data store during its entire retention period\. This metric is updated nightly\. | Bytes | 

For more information about CloudWatch metrics, see the following topics\.
+  [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) 
+  [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) 