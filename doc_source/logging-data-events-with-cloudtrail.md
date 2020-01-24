# Logging Data Events for Trails<a name="logging-data-events-with-cloudtrail"></a>

By default, trails do not log data events\. Additional charges apply for data events\. For more information, see [ AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

**Note**  
The events that are logged by your trails are available in Amazon CloudWatch Events\. For example, if you configure a trail to log data events for S3 objects but not management events, your trail processes and logs only data events for the specified S3 objects\. The data events for these S3 objects are available in Amazon CloudWatch Events\. For more information, see [AWS API Call Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#api_event_type) in the *Amazon CloudWatch Events User Guide*\. 

**Contents**
+ [Data Events](#logging-data-events)
  + [Logging Data Events with the AWS Management Console](#logging-data-events-with-the-cloudtrail-console)
  + [Examples: Logging Data Events for Amazon S3 Objects](#logging-data-events-examples)
  + [Logging Data Events for S3 Objects in Other AWS Accounts](#logging-data-events-for-s3-resources-in-other-accounts)
+ [Read\-only and Write\-only Events](#read-write-events-data)
+ [Logging Events with the AWS Command Line Interface](#creating-data-event-selectors-with-the-AWS-CLI)
+ [Logging Events with the AWS SDKs](#logging-data-events-with-the-AWS-SDKs)
+ [Sending Events to Amazon CloudWatch Logs](#sending-data-events-to-cloudwatch-logs)

## Data Events<a name="logging-data-events"></a>

Data events provide visibility into the resource operations performed on or within a resource\. These are also known as data plane operations\. Data events are often high\-volume activities\. 

 Example data events include:
+  Amazon S3 object\-level API activity \(for example, `GetObject`, `DeleteObject`, and `PutObject` API operations\)
+ AWS Lambda function execution activity \(the `Invoke` API\)

 Data events are disabled by default when you create a trail\. To record CloudTrail data events, you must explicitly add the supported resources or resource types for which you want to collect activity to a trail\. For more information, see [Creating a Trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Data Events](#logging-data-events)\.

Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

### Logging Data Events with the AWS Management Console<a name="logging-data-events-with-the-cloudtrail-console"></a>

1. Navigate to the **Trails** page of the CloudTrail console and choose **Create trail**\. 
**Note**  
While you can edit an existing trail to add logging data events, as a best practice, consider creating a separate trail specifically for logging data events\.

1. For **Data events**, choose the pencil icon to enable editing\. 

1. For Amazon S3 data events, on the **S3** tab:

   1. To configure data event logging for all Amazon S3 buckets in your AWS account, select **Select all S3 buckets in your account**\. Then choose whether you want to log **Read** events, such as `GetObject`; **Write** events, such as `PutObject`; or both types of events\. This setting takes precedence over any settings you configure for individual buckets\. For example, if you specify logging **Read** events for all S3 buckets, and then choose to add a specific bucket for data event logging, **Read** will already be selected for that bucket\. You cannot clear the selection\. You can only configure the option for **Write**\. 
**Note**  
If you select or clear an option for all buckets, that change is applied to all buckets you might have individually configured for data event logging\. Consider reviewing the data event settings for individual buckets after you change the data event settings for all buckets\.   
If you configure data event logging for all buckets in your AWS account, and you do not want an audit trail of data event logging, consider delivering your log files to an Amazon S3 bucket that belongs to another AWS account\. For more information, see [Receiving CloudTrail Log Files from Multiple Accounts](cloudtrail-receive-logs-from-multiple-accounts.md) and [Examples: Logging Data Events for Amazon S3 Objects](#logging-data-events-examples)\.

   1. To configure data event logging for individual Amazon S3 buckets, choose **Add S3 bucket**\. Type the bucket name and prefix \(optional\)\. For each trail, you can add up to 250 data resources, such as Amazon S3 bucket and object prefixes\. The overall total of individual data event resources cannot exceed 250 in a single trail\. That total includes other data resources, such as Lambda functions\. This restriction does not apply if you configure data event logging for all Amazon S3 buckets\.
      + To log data events for all S3 objects in a bucket, specify an S3 bucket and an empty prefix\. When an event occurs on an object in that S3 bucket, the trail processes and logs the event\. For more information, see [Example: Logging data events for all S3 objects](#example-logging-all-S3-objects)\.
      + To log data events for S3 prefixes, specify an S3 bucket and the object prefix\. When an event occurs on an object in that S3 bucket and the object starts with the specified prefix, the trail processes and logs the event\. For more information, see [Example: Logging data events for specific S3 objects](#example-logging-specific-objects)\. 
      + You can also specify S3 objects that belong to other AWS accounts\. For more information, see [Logging Data Events for S3 Objects in Other AWS Accounts](#logging-data-events-for-s3-resources-in-other-accounts)\.

   1. For each resource, specify whether you want to log **Read**, **Write**, or both types of events\.

   1. You can edit the bucket name, prefix, **Read/Write** option, or remove the resource by choosing the **x** icon\.
**Note**  
If you configured data event logging for all S3 buckets in your AWS account, the settings you configured take precedence over individual bucket settings\. In this case, you cannot edit an option that is set for all buckets\.

1. For Lambda data events, on the **Lambda** tab:

   1. To configure data event logging for individual Lambda functions, select them from the list\. If the trail applies to all AWS Regions, you can select from functions in all regions in your AWS account\. If the trail applies to only one region, you can only select from functions in that region\. For each trail, you can add up to 250 data resources, such as individual Lambda functions\. The total of individual data event resources cannot exceed 250 in a single trail\. That total includes other data resources, such as Amazon S3 bucket and object prefixes\. This restriction does not apply if you configure data event logging for all Lambda functions\.
**Note**  
If you have more than 15,000 Lambda functions in your account, you cannot view or select all functions in the CloudTrail console\. You can still select the option to log all Lambda functions\. You can also manually add a function if you know its ARN, or you can use the AWS CLI and the put\-event\-selectors command to configure specific data event logging for resources\. For more information, see [Managing Trails With the AWS CLI](cloudtrail-additional-cli-commands.md)\.

   1. To configure data event logging for all Lambda functions in your AWS account, and any Lambda function you might create in the future, select **Log all current and future functions**\. If the trail applies to all regions, this will log all functions in all regions in your AWS account, including any you might create in any region\. If the trail applies to a single region, this will log all functions in the current region and any you might create in that region, but will not enable logging of functions in other regions\. Logging data events for all functions will also enable logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a function that belongs to another AWS account\.

1. Choose **Save**\.

### Examples: Logging Data Events for Amazon S3 Objects<a name="logging-data-events-examples"></a>

**Logging data events for all S3 objects in an S3 bucket**

The following example demonstrates how logging works when you configure logging of all data events for an S3 bucket named *bucket\-1*\. In this example, the CloudTrail user specified an empty prefix, and the option to log both **Read** and **Write** data events\. 

1. A user uploads an object to `bucket-1`\. 

1. The `PutObject` API operation is an Amazon S3 object\-level API\. It is recorded as a data event in CloudTrail\. Because the CloudTrail user specified an S3 bucket with an empty prefix, events that occur on any object in that bucket are logged\. The trail processes and logs the event\.

1. Another user uploads an object to `bucket-2`\. 

1. The `PutObject` API operation occurred on an object in an S3 bucket that wasn't specified for the trail\. The trail doesn't log the event\. 

**Logging data events for specific S3 objects**

The following example demonstrates how logging works when you configure a trail to log events for specific S3 objects\. In this example, the CloudTrail user specified an S3 bucket named *bucket\-3*, with the prefix *my\-images*, and the option to log only **Write** data events\.

1. A user deletes an object that begins with the `my-images` prefix in the bucket, such as `arn:aws:s3:::bucket-3/my-images/example.jpg`\.

1. The `DeleteObject` API operation is an Amazon S3 object\-level API\. It is recorded as a **Write** data event in CloudTrail\. The event occurred on an object that matches the S3 bucket and prefix specified in the trail\. The trail processes and logs the event\.

1. Another user deletes an object with a different prefix in the S3 bucket, such as `arn:aws:s3:::bucket-3/my-videos/example.avi`\.

1. The event occurred on an object that doesn't match the prefix specified in your trail\. The trail doesn't log the event\.

1. A user calls the `GetObject` API operation for the object, `arn:aws:s3:::bucket-3/my-images/example.jpg`\.

1. The event occurred on a bucket and prefix that are specified in the trail, but `GetObject` is a read\-type Amazon S3 object\-level API\. It is recorded as a **Read** data event in CloudTrail, and the trail is not configured to log **Read** events\. The trail doesn't log the event\.

**Note**  
If you are logging data events for specific Amazon S3 buckets, we recommend you do not use an Amazon S3 bucket for which you are logging data events to receive log files that you have specified in the data events section\. Using the same Amazon S3 bucket causes your trail to log a data event each time log files are delivered to your Amazon S3 bucket\. Log files are aggregated events delivered at intervals, so this is not a 1:1 ratio of event to log file; the event is logged in the next log file\. For example, when the trail delivers logs, the `PutObject` event occurs on the S3 bucket\. If the S3 bucket is also specified in the data events section, the trail processes and logs the `PutObject` event as a data event\. That action is another `PutObject` event, and the trail processes and logs the event again\. For more information, see [How CloudTrail Works](how-cloudtrail-works.md)\.  
To avoid logging data events for the Amazon S3 bucket where you receive log files if you configure a trail to log all Amazon S3 data events in your AWS account, consider configuring delivery of log files to an Amazon S3 bucket that belongs to another AWS account\. For more information, see [Receiving CloudTrail Log Files from Multiple Accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.

### Logging Data Events for S3 Objects in Other AWS Accounts<a name="logging-data-events-for-s3-resources-in-other-accounts"></a>

When you configure your trail to log data events, you can also specify S3 objects that belong to other AWS accounts\. When an event occurs on a specified object, CloudTrail evaluates whether the event matches any trails in each account\. If the event matches the settings for a trail, the trail processes and logs the event for that account\. Generally, both API callers and resource owners can receive events\.

If you own an S3 object and you specify it in your trail, your trail logs events that occur on the object in your account\. Because you own the object, your trail also logs events when other accounts call the object\.

If you specify an S3 object in your trail, and another account owns the object, your trail only logs events that occur on that object in your account\. Your trail doesn't log events that occur in other accounts\.

**Example: Logging data events for an S3 object for two AWS accounts**

The following example shows how two AWS accounts configure CloudTrail to log events for the same S3 object\.

1. In your account, you want your trail to log data events for all objects in your S3 bucket named `owner-bucket`\. You configure the trail by specifying the S3 bucket with an empty object prefix\.

1. Bob has a separate account that has been granted access to the S3 bucket\. Bob also wants to log data events for all objects in the same S3 bucket\. For his trail, he configures his trail and specifies the same S3 bucket with an empty object prefix\.

1. Bob uploads an object to the S3 bucket with the `PutObject` API operation\.

1. This event occurred in his account and it matches the settings for his trail\. Bob's trail processes and logs the event\.

1. Because you own the S3 bucket and the event matches the settings for your trail, your trail also processes and logs the same event\. Because there are now two copies of the event \(one logged in Bob's trail, and one logged in yours\), CloudTrail charges for two copies of the data event\.

1. You upload an object to the S3 bucket\.

1. This event occurs in your account and it matches the settings for your trail\. Your trail processes and logs the event\.

1. Because the event didn't occur in Bob's account, and he doesn't own the S3 bucket, Bob's trail doesn't log the event\. CloudTrail charges for only one copy of this data event\.

**Example: Logging data events for all buckets, including an S3 bucket used by two AWS accounts**

The following example shows the logging behavior when **Select all S3 buckets in your account** is enabled for trails that collect data events in an AWS account\.

1. In your account, you want your trail to log data events for all S3 buckets\. You configure the trail by choosing **Select all S3 buckets in your account** in **Data events**\.

1. Bob has a separate account that has been granted access to an S3 bucket in your account\. He wants to log data events for the bucket to which he has access\. He configures his trail to get data events for all S3 buckets\.

1. Bob uploads an object to the S3 bucket with the `PutObject` API operation\.

1. This event occurred in his account and it matches the settings for his trail\. Bob's trail processes and logs the event\.

1. Because you own the S3 bucket and the event matches the settings for your trail, your trail also processes and logs the event\. Because there are now two copies of the event \(one logged in Bob's trail, and one logged in yours\), CloudTrail charges each account for a copy of the data event\.

1. You upload an object to the S3 bucket\.

1. This event occurs in your account and it matches the settings for your trail\. Your trail processes and logs the event\.

1. Because the event didn't occur in Bob's account, and he doesn't own the S3 bucket, Bob's trail doesn't log the event\. CloudTrail charges for only one copy of this data event in your account\.

1. A third user, Mary, has access to the S3 bucket, and runs a `GetObject` operation on the bucket\. She has a trail configured to log data events on all S3 buckets in her account\. Because she is the API caller, CloudTrail logs a data event in her trail\. Though Bob has access to the bucket, he is not the resource owner, so no event is logged in his trail this time\. As the resource owner, you receive an event in your trail about the `GetObject` operation that Mary called\. CloudTrail charges your account and Mary's account for each copy of the data event: one in Mary's trail, and one in yours\.

## Read\-only and Write\-only Events<a name="read-write-events-data"></a>

When you configure your trail to log data and management events, you can specify whether you want read\-only events, write\-only events, both, or none\. Insights events log only activity based on write\-only events\.
+ **Read\-only**

  Read\-only events include API operations that read your resources, but don't make changes\. For example, read\-only events include the Amazon EC2 `DescribeSecurityGroups` and `DescribeSubnets` API operations\. These operations return only information about your Amazon EC2 resources and don't change your configurations\. 
+ **Write\-only**

  Write\-only events include API operations that modify \(or might modify\) your resources\. For example, the Amazon EC2 `RunInstances` and `TerminateInstances` API operations modify your instances\.
+ **All**

  Your trail logs both\.
+ **None**

  Your trail logs neither read\-only nor write\-only management events\.

**Example: Logging read\-only and write\-only events for separate trails**

The following example shows how you can configure trails to split log activity for an account into separate S3 buckets: one bucket receives read\-only events and a second bucket receives write\-only events\. 

1. You create a trail and choose an S3 bucket named `read-only-bucket` to receive log files\. You then update the trail to specify that you want read\-only management events and data events\. 

1. You create a second trail and choose an S3 bucket named `write-only-bucket` to receive log files\. You then update the trail to specify that you want write\-only management events and data events\.

1. The Amazon EC2 `DescribeInstances` and `TerminateInstances` API operations occur in your account\.

1. The `DescribeInstances` API operation is a read\-only event and it matches the settings for the first trail\. The trail logs and delivers the event to the `read-only-bucket`\.

1. The `TerminateInstances` API operation is a write\-only event and it matches the settings for the second trail\. The trail logs and delivers the event to the `write-only-bucket`\.

## Logging Events with the AWS Command Line Interface<a name="creating-data-event-selectors-with-the-AWS-CLI"></a>

You can configure your trails to log management and data events using the AWS CLI\.

To view whether your trail is logging management and data events, run the `get-event-selectors` command\. 

```
aws cloudtrail get-event-selectors --trail-name TrailName
```

The following example returns the default settings for a trail\. By default, trails log all management events and don't log data events\.

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

To configure your trail to log management and data events, run the `put-event-selectors` command\. The following example shows how to configure your trail to include all management and data events for two S3 objects\. You can specify from 1 to 5 event selectors for a trail\. You can specify from 1 to 250 data resources for a trail\. 

**Note**  
The maximum number of S3 data resources is 250, regardless of the number of event selectors\. 

```
aws cloudtrail put-event-selectors --trail-name TrailName --event-selectors '[{ "ReadWriteType": "All", "IncludeManagementEvents":true, "DataResources": [{ "Type": "AWS::S3::Object", "Values": ["arn:aws:s3:::mybucket/prefix", "arn:aws:s3:::mybucket2/prefix2"] }] }]'
```

The following example returns the event selector configured for the trail\.

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [
                {
                    "Values": [
                        "arn:aws:s3:::mybucket/prefix",
                        "arn:aws:s3:::mybucket2/prefix2",
                    ],
                    "Type": "AWS::S3::Object"
                }
            ],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

## Logging Events with the AWS SDKs<a name="logging-data-events-with-the-AWS-SDKs"></a>

Use the [GetEventSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_GetEventSelectors.html) operation to see whether your trail is logging data events for a trail\. You can configure your trails to log data events with the [PutEventSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutEventSelectors.html) operation\. For more information, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

## Sending Events to Amazon CloudWatch Logs<a name="sending-data-events-to-cloudwatch-logs"></a>

CloudTrail supports sending data events to CloudWatch Logs\. When you configure your trail to send events to your CloudWatch Logs log group, CloudTrail sends only the events that you specify in your trail\. For example, if you configure your trail to log data events only, your trail delivers data events only to your CloudWatch Logs log group\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\. 