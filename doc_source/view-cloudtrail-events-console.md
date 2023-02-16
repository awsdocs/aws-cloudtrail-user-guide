# Viewing CloudTrail events in the CloudTrail console<a name="view-cloudtrail-events-console"></a>

You can use the CloudTrail console to view the last 90 days of recorded API activity \(management events\) in an AWS Region\. You can also download a file with that information, or a subset of information based on the filter and time range you choose\. You can customize your view of **Event history** by selecting which columns are displayed in the console\. You can also look up and filter events by the resource types available for a particular service\. You can select up to five events in **Event history** and compare their details side\-by\-side\.

**Event history** does not show data events\. To view data events, [create a trail](cloudtrail-create-and-update-a-trail.md)\.

After 90 days, events are no longer shown in **Event history**\. You cannot manually delete events from **Event history**\. When you [create a trail](cloudtrail-create-a-trail-using-the-console-first-time.md), you can view events that are logged to your trail for as long as you store them in the S3 bucket that is configured in your trail settings\.

CloudTrail logging varies between AWS services\. While most AWS services support CloudTrail logging of all events, some services only support logging a subset of APIs and events, and a few services are unsupported\. You can learn more about the specifics of how CloudTrail logs events for a specific service by consulting the documentation for that service\. For more information, see [CloudTrail supported services and integrations](cloudtrail-aws-service-specific-topics.md)\.

**Note**  
For an ongoing record of activity and events, [create a trail](cloudtrail-create-a-trail-using-the-console-first-time.md)\. Creating a trail also enables you to take advantage of the following integrations:  
A trail lets you log CloudTrail Insights events, which can help you identify and respond to unusual activity associated with `write` management API calls\. For more information, see [Logging Insights events for trails](logging-insights-events-with-cloudtrail.md)\.
Analyze your AWS service activity with queries in Amazon Athena\. For more information, see [Creating a Table for CloudTrail Logs in the CloudTrail Console](https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html#create-cloudtrail-table-ct) in the [Amazon Athena User Guide](https://docs.aws.amazon.com/athena/latest/ug/), or choose the option to create a table directly from **Event history** in the CloudTrail console\.
Monitor your trail logs and be notified when specific activity occurs with Amazon CloudWatch Logs\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.
A trail lets you exclude AWS Key Management Service \(AWS KMS\) or Amazon Relational Database Service Data API events\. AWS KMS actions such as `Encrypt`, `Decrypt`, and `GenerateDataKey` typically generate a large volume \(more than 99%\) of events\. Events cannot be excluded from **Event history**; you can only exclude events if you create or update a trail to log management events\.

**To view CloudTrail events**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/home/](https://console.aws.amazon.com/cloudtrail/home/)\.

1. In the navigation pane, choose **Event history**\. 

   A filtered list of events appears in the content pane with the latest event first\. Scroll down to see more events\.

1. To compare events, select up to five events by filling their check boxes in the left margin of the **Event history** table\. View details for selected events side\-by\-side in the **Compare event details** table\.

The default view of events in **Event history** has a filter applied so that it does not display read\-only events\. To remove this filter, or to apply other filters, change the filter settings\. For more information, see [Filtering CloudTrail events](#filtering-cloudtrail-events)\.



**Contents**
+ [Displaying CloudTrail events](#displaying-cloudtrail-events)
+ [Filtering CloudTrail events](#filtering-cloudtrail-events)
+ [Viewing details for an event](#viewing-details-for-an-event)
+ [Downloading events](#downloading-events)
+ [Viewing resources referenced with AWS Config](#viewing-resources-config)

## Displaying CloudTrail events<a name="displaying-cloudtrail-events"></a>

You can customize the display of **Event history** by selecting which columns to display in the CloudTrail console\. By default, the following columns are displayed:
+ **Event name**
+ **Event time**
+ **User name**
+ **Event source**
+ **Resource type**
+ **Resource name**

**Note**  
You cannot change the order of the columns, or manually delete events from **Event history**\.

**To customize the columns displayed in **Event history****

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/home/](https://console.aws.amazon.com/cloudtrail/home/)\.

1. In the navigation pane, choose **Event history**\. 

1. Choose the gear icon\.

1. In **Select visible columns**, select the columns you want to display\. Turn off columns you do not want to display\. When you have finished, choose **Confirm**\.

## Filtering CloudTrail events<a name="filtering-cloudtrail-events"></a>

The default display of events in **Event history** uses an attribute filter to exclude read\-only events from the list of displayed events\. This attribute filter is named **Read\-only**, and it is set to **false**\. You can remove this filter to display both read and write events\. To view only **Read** events, you can change the filter value to **true**\. You can also filter events by other attributes\. You can additionally filter by time range\.

**Note**  
You can only apply one attribute filter and a time range filter\. You cannot apply multiple attribute filters\.

** AWS access key **  
The AWS access key ID that was used to sign the request\. If the request was made with temporary security credentials, this is the access key ID of the temporary credentials\.

** Event ID **  
The CloudTrail ID of the event\. Each event has a unique ID\.

** Event name **  
The name of the event\. For example, you can filter on IAM events, such as `CreatePolicy`, or Amazon EC2 events, such as `RunInstances`\.

** Event source **  
The AWS service to which the request was made, such as `iam.amazonaws.com` or `s3.amazonaws.com`\. You can scroll through a list of event sources after you choose the **Event source** filter\. 

** Read only **  
The read type of the event\. Events are categorized as read events or write events\. If set to **false**, read events are not included in the list of displayed events\. By default, this attribute filter is applied and the value is set to **false**\.

** Resource name **  
The name or ID of the resource referenced by the event\. For example, the resource name might be "auto\-scaling\-test\-group" for an Auto Scaling group or "i\-12345678910" for an EC2 instance\.

** Resource type **  
The type of resource referenced by the event\. For example, a resource type can be `Instance` for EC2 or `DBInstance` for RDS\. Resource types vary for each AWS service\. 

** Time range **  
The time range in which you want to filter events\. You can filter events for the last 90 days\.

** User name **  
The identity referenced by the event\. For example, this can be a user, a role name, or a service role\.

If there are no events logged for the attribute or time that you choose, the results list is empty\. You can apply only one attribute filter in addition to the time range\. If you choose a different attribute filter, your specified time range is preserved\.

The following steps describe how to filter by attribute\. 

**To filter by attribute**

1. To filter the results by an attribute, choose an attribute from the **Lookup attributes** drop\-down list, and then type or choose a value for the attribute in the text box\.

1. To remove an attribute filter, choose the **X** at the right of the attribute filter box\.

The following steps describe how to filter by a start and end date and time\.

**To filter by a start and end date and time**

1. To narrow the time range for the events that you want to see, choose a time range in the time range bar\. Preset values are 30 minutes, 1 hour, 3 hours, or 12 hours\. To specify a custom time range, choose **Custom**\.

1. To remove a time range filter, choose **Clear** in the time range bar\.

## Viewing details for an event<a name="viewing-details-for-an-event"></a>

1. Choose an event in the results list to show its details\.

1. Resources referenced in the event are shown in the **Resources referenced** table on the event details page\.

1. Some referenced resources have links\. Choose the link to open the console for that resource\.

1. Scroll to **Event record** on the details page to see the JSON event record, also called the event *payload*\.

1. Choose **Event history** in the page breadcrumb to close the event details page and return to **Event history**\.

## Downloading events<a name="downloading-events"></a>

You can download recorded event history as a file in CSV or JSON format\. Use filters and time ranges to reduce the size of the file you download\. 

**Note**  
CloudTrail event history files are data files that contain information \(such as resource names\) that can be configured by individual users\. Some data can potentially be interpreted as commands in programs used to read and analyze this data \(CSV injection\)\. For example, when CloudTrail events are exported to CSV and imported to a spreadsheet program, that program might warn you about security concerns\. You should choose to disable this content to keep your system secure\. Always disable links or macros from downloaded event history files\.

1. Add a filter and time range for events in **Event history** that you want to download\. For example, you can specify the event name, `StartInstances`, and specify a time range for the last three days of activity\.

1. Choose **Download events**, and then choose **Download as CSV** or **Download as JSON**\. The download starts immediately\.
**Note**  
Your download might take some time to complete\. For faster results, before you start the download process, use a more specific filter or a shorter time range to narrow the results\. You can cancel a download\. If you cancel a download, a partial download including only some event data might be on your local computer\. To download the full event history, restart the download\.

1. After your download is complete, open the file to view the events that you specified\.

1. To cancel your download, choose **Cancel**, and then confirm by choosing **Cancel download**\. If you need to restart a download, wait until the earlier download is finished canceling\.

## Viewing resources referenced with AWS Config<a name="viewing-resources-config"></a>

AWS Config records configuration details, relationships, and changes to your AWS resources\. 

On the **Resources Referenced** pane, choose the ![\[AWS Config timeline icon\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/config-timeline.png) in the **Config timeline** column to view the resource in the AWS Config console\.

If the ![\[AWS Config timeline\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/config-timeline-gray.png) icon is gray, AWS Config isn't turned on, or it's not recording the resource type\. Choose the icon to go to the AWS Config console to turn on the service or start recording that resource type\. For more information, see [Set Up AWS Config Using the Console](https://docs.aws.amazon.com/config/latest/developerguide/gs-console.html) in the *AWS Config Developer Guide*\.

If **Link not available** appears in the column, the resource can't be viewed for one of the following reasons:
+ AWS Config doesn't support the resource type\. For more information, see [Supported Resources, Configuration Items, and Relationships](https://docs.aws.amazon.com/config/latest/developerguide/resource-config-reference.html) in the *AWS Config Developer Guide*\.
+ AWS Config recently added support for the resource type, but it's not yet available from the CloudTrail console\. You can look up the resource in the AWS Config console to see the timeline for the resource\.
+ The resource is owned by another AWS account\.
+ The resource is owned by another AWS service, such as a managed IAM policy\.
+ The resource was created and then deleted immediately\.
+ The resource was recently created or updated\.

**Example**  

1. You configure AWS Config to record IAM resources\.

1. You create an IAM user, **Bob\-user**\. The **Event history** page shows the `CreateUser` event and **Bob\-user** as an IAM resource\. You can choose the AWS Config icon to view this IAM resource in the AWS Config timeline\.

1. You update the user name to **Bob\-admin**\.

1. The **Event history** page shows the `UpdateUser` event and **Bob\-admin** as the updated IAM resource\.

1. You can choose the icon to view the **Bob\-admin** IAM resource in the timeline\. However, you can't choose the icon for **Bob\-user**, because the resource name changed\. AWS Config is now recording the updated resource\.

To grant users read\-only permission to view resources in the AWS Config console, see [Granting permission to view AWS Config information on the CloudTrail console](security_iam_id-based-policy-examples.md#grant-aws-config-permissions-for-cloudtrail-users)\.

For more information about AWS Config, see the [AWS Config Developer Guide](https://docs.aws.amazon.com/config/latest/developerguide/)\.