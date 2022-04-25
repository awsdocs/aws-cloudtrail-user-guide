# Create an event data store<a name="query-event-data-store"></a>

To get started with CloudTrail Lake, create an event data store\. An event data store can include events that you have logged on your account from the last 7 to 2555 days \(a week to up to seven years\)\. By default, event data is retained for 2555 days, and termination protection is enabled for an event data store\.

1. Choose **Lake** in the left navigation pane of the CloudTrail console\.

1. On the **Lake** page, open the **Event data stores** tab\.

1. Choose **Create event data store**\.

1. On the **Configure event data store** page, in **General details**, enter a name for the event data store\. A name is required\.

1. Specify a retention period for the event data store in days\. Valid values are integers from 7 to 2555 \(seven years\)\. The event data store retains the specified number of days worth of events that are logged by CloudTrail\. For example, if you specify a retention period of 100 days, a query run on this data store queries events that have been logged within the last 100 days before you ran the query\.

1. CloudTrail stores the event data store resource in the region in which you create it, but by default, the events collected in the data store are from all regions in your account\. Optionally, you can select **Include only the current region in my event data store** to include only events that are logged in the current region\. If you do not choose this option, your event data store includes events from all regions\. Choose **Next**\.

1. To have your event data store collect events from all accounts in an AWS Organizations organization, select **Enable for all accounts in my organization**\. You must be signed in to the management account for the organization to create an event data store that collects events for an organization\.

1. \(Optional\) In the **Tags** area, you can add up to 50 tag key and value pairs to help you identify, sort, and control access to your event data store\. For more information about how to use IAM policies to authorize access to an event data store based on tags, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags)\. For more information about how you can use tags in AWS, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.

1. On the **Choose events** page, choose at least one event type\. By default, **Management events** is selected\. You can add both management and data events to your event data store\. For more information about management events, see [Logging management events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-management-events-with-cloudtrail.html)\. For more information about data events, see [Logging data events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)\.

1. If your event data store includes management events, choose **Read**, **Write**, or both\. At least one is required\. For more information about **Read** and **Write** management events, see [Logging management events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-management-events-with-cloudtrail.html)\.

1. You can choose to exclude AWS Key Management Service or Amazon RDS Data API events from your event data store\. For more information about these options, see [Logging management events for trails](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-management-events-with-cloudtrail.html)\.

1. To include data events in your event data store, do the following\.

   1. Choose a data event type\. This is the AWS service and resource on which data events are logged\.

   1. In **Log selector template**, choose a template\. You can choose to log all data events, `readOnly` events, `writeOnly` events, or **Custom** to build a custom log selector\.

   1. If you choose **Custom**, optionally enter a name for your custom log selector template\.

   1. In **Advanced event selectors**, build expressions by choosing values for **Field**, **Operator**, and **Value**\. Advanced event selectors for an event data store work the same as advanced event selectors that you apply to a trail\. For more information about how to build advanced event selectors, see [Logging data events with advanced event selectors](logging-data-events-with-cloudtrail.md#creating-data-event-selectors-advanced)\.

      The following example uses a **Custom** log selector template to choose only event names from S3 objects that start with `Put`, such as `PutObject`\. Because the advanced event selector does not include or exclude any other event types or resource ARNs, all S3 data events, both read and write, that are logged to the US East \(N\. Virginia\) Region, and that have event names starting with `Put`, are stored in the event data store\.  
![\[Lake data events, advanced event selectors\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-data-events.png)
**Important**  
To exclude or include data events with advanced event selectors by using an S3 bucket ARN, always use the **Starts with** operator\.

   1. Optionally, expand **JSON view** to see your advanced event selectors as a JSON block\. Choose **Next**\.

1. On the **Review and create** page, review your choices\. Choose **Edit** to make changes to a section\. When you're ready to create the event data store, choose **Create event data store**\.

1. The new event data store is visible in the **Event data stores** table on the **Lake** page\.

   From this point forward, the event data store captures events that match its advanced event selectors\. Events that occurred before you created the event data store are not in the event data store\.