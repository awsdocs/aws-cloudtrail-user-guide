# Configuring CloudTrail to Send Notifications<a name="configure-cloudtrail-to-send-notifications"></a>

You can configure a trail to use an Amazon SNS topic\. You can use the CloudTrail console or the [http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/create-subscription.html](http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/create-subscription.html) CLI command to create the topic\. CloudTrail creates the Amazon SNS topic for you and attaches an appropriate policy, so that CloudTrail has permission to publish to that topic\. 

When you create an SNS topic name, the name must meet the following requirements:

+ Between 1 and 256 characters long

+ Contain uppercase and lowercase ASCII letters, numbers, underscores, or hyphens 

When you configure notifications for a trail that applies to all regions, notifications from all regions are sent to the Amazon SNS topic that you specify\. If you have one or more region\-specific trails, you must create a separate topic for each region and subscribe to each individually\. 

To receive notifications, subscribe to the Amazon SNS topic or topics that CloudTrail uses\. You do this with the Amazon SNS console or Amazon SNS CLI commands\. For more information, see [Subscribe to a Topic](http://docs.aws.amazon.com/sns/latest/dg/SubscribeTopic.html) in the *Amazon Simple Notification Service Developer Guide*\. 

**Note**  
CloudTrail sends a notification when log files are written to the Amazon S3 bucket\. An active account can generate a large number of notifications\. If you subscribe with email or SMS, you can receive a large volume of messages\. We recommend that you subscribe using Amazon Simple Queue Service \(Amazon SQS\), which lets you handle notifications programmatically\. For more information, see [Subscribing a Queue to an Amazon SNS Topic](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqssubscribe.html) in the *Amazon Simple Queue Service Developer Guide*\. 

The Amazon SNS notification consists of a JSON object that includes a `Message` field\. The `Message` field lists the full path to the log file, as shown in the following example: 

```
{
    "s3Bucket": "your-bucket-name","s3ObjectKey": ["AWSLogs/123456789012/CloudTrail/us-east-2/2013/12/13/123456789012_CloudTrail_us-west-2_20131213T1920Z_LnPgDQnpkSKEsppV.json.gz"]
}
```

If multiple log files are delivered to your Amazon S3 bucket, a notification may contain multiple logs, as shown in the following example:

```
{
    "s3Bucket": "your-bucket-name",
    "s3ObjectKey": [
        "AWSLogs/123456789012/CloudTrail/us-east-2/2016/08/11/123456789012_CloudTrail_us-east-2_20160811T2215Z_kpaMYavMQA9Ahp7L.json.gz",
        "AWSLogs/123456789012/CloudTrail/us-east-2/2016/08/11/123456789012_CloudTrail_us-east-2_20160811T2210Z_zqDkyQv3TK8ZdLr0.json.gz",
        "AWSLogs/123456789012/CloudTrail/us-east-2/2016/08/11/123456789012_CloudTrail_us-east-2_20160811T2205Z_jaMVRa6JfdLCJYHP.json.gz"
    ]
}
```

If you choose to receive notifications by email, the body of the email consists of the content of the `Message` field\. For a complete description of the JSON structure, see [Sending Amazon SNS Messages to Amazon SQS Queues](http://docs.aws.amazon.com/sns/latest/dg/SendMessageToSQS.html) in the *Amazon Simple Notification Service Developer Guide*\. Only the `Message` field shows CloudTrail information\. The other fields contain information from the Amazon SNS service\. 

If you create a trail with the CloudTrail API, you can specify an existing Amazon SNS topic that you want CloudTrail to send notifications to with the [http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateTrail.html) or [http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) operations\. You must make sure that the topic exists and that it has permissions that allow CloudTrail to send notifications to it\. See [Amazon SNS Topic Policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)\. 

## Additional Resources<a name="cloudtrail-notifications-more-info-4"></a>

For more information about Amazon SNS topics and about subscribing to them, see the [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/)\.