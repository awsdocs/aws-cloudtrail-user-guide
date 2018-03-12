# Configuring Amazon SNS Notifications for CloudTrail<a name="configure-sns-notifications-for-cloudtrail"></a>

 You can be notified when CloudTrail publishes new log files to your Amazon S3 bucket\. You manage notifications using Amazon Simple Notification Service \(Amazon SNS\)\.

Notifications are optional\. If you want notifications, you configure CloudTrail to send update information to an Amazon SNS topic whenever a new log file has been sent\. To receive these notifications, you can use Amazon SNS to subscribe to the topic\. As a subscriber you can get updates sent to a Amazon Simple Queue Service \(Amazon SQS\) queue, which enables you to handle these notifications programmatically\. 


+ [Configuring CloudTrail to Send Notifications](configure-cloudtrail-to-send-notifications.md)
+ [Amazon SNS Topic Policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)