# Updating a Trail<a name="cloudtrail-update-a-trail-console"></a>

To change trail settings, use the following procedure\.

**To update a trail with the AWS Management Console**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Trails** and then choose a trail\.

1. To make updates for **Trail settings**, choose the pencil icon, specify if you want your trail to apply to a single region or all regions, and then choose **Save**\.

1. For **Management events**, do the following\.

   1. For **Read/Write events**, choose if you want your trail to log **All**, **Read\-only**, **Write\-only**, or **None**, and then choose **Save**\. By default, trails log all management events\. For more information, see [Management Events](logging-management-events-with-cloudtrail.md#logging-management-events)\.

   1. For **Log AWS KMS events**, choose **Yes** to log AWS Key Management Service \(AWS KMS\) events in your trail\. Choose **No** to filter AWS KMS events out of your trail\. The default setting is **Yes**\.

1. In **Insights events**, for **Log Insights events**, choose **Yes** if you want your trail to log Insights events\. By default, trails don't log Insights events\. For more information about Insights events, see [Logging Insights Events for Trails](logging-insights-events-with-cloudtrail.md)\. Additional charges apply for logging Insights events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

   Insights events are delivered to a different folder named `/CloudTrail-Insight` of the same S3 bucket that is specified in the **Storage location** area of the trail details page\. CloudTrail creates the new prefix for you\. For example, if your current destination S3 bucket is named `S3bucketName/AWSLogs/CloudTrail/`, the S3 bucket name with a new prefix is named `S3bucketName/AWSLogs/CloudTrail-Insight/`\.

1. For **Data events**, choose the pencil icon or **Configure**, make your changes, and then choose **Save**\. By default, trails don't log data events\. For more information, see [Data Events](logging-data-events-with-cloudtrail.md#logging-data-events)\.

1. For **Storage location**, choose the pencil icon to update the settings for the following: 
   + The S3 bucket \(with optional prefix\) that is receiving your log files\.
   + Log file encryption with AWS KMS\.
   + Log file validation for logs\.
   + The Amazon SNS topic to notify you when log files are delivered\.

   For more information, see [Configuring Advanced Settings for Your Trail](cloudtrail-create-a-trail-using-the-console-first-time.md#advanced-settings-for-your-trail)\.

1. Choose **Save**\.

**To configure CloudWatch Logs and tags for your trail**

1. To configure CloudTrail to deliver events to CloudWatch Logs for monitoring, for **CloudWatch Logs **, choose **Configure**\. For more information about these settings, see [Sending Events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\. 

1. To configure tags \(custom key\-value pairs\) for your trail, for **Tags**, click the pencil icon\. You can add up to 50 key\-value pairs per trail\. Trail tags must be configured from the region in which the trail was created\. 

1. When finished, choose **Apply**\.