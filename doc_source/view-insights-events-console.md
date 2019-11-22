# Viewing CloudTrail Insights Events in the CloudTrail Console<a name="view-insights-events-console"></a>

After you enable CloudTrail Insights events on a trail, when CloudTrail detects unusual write management API activity, CloudTrail generates Insights events and displays them on the **Dashboard** and **Insights** pages in the AWS Management Console\. You can view the Insights events in the console and troubleshoot the unusual activity\. The most recent 90 days of Insights events are shown in the console\. You can also download Insights events by using the AWS CloudTrail console\. You can programmatically look up events by using the AWS SDKs or AWS Command Line Interface\. For more information about CloudTrail Insights events, see [Logging Insights Events for Trails](logging-insights-events-with-cloudtrail.md) in this guide\.

After Insights events are logged, the events are shown on the **Insights** page for 90 days\. You cannot manually delete events from the **Insights** page\. Because you must [create a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) before you can enable CloudTrail Insights, you can view Insights events that are logged to your trail for as long as you store them in the S3 bucket that is configured in your trail settings\.

Monitor your trail logs and be notified when specific Insights events activity occurs with Amazon CloudWatch Logs\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

**To view Insights events**

CloudTrail Insights events must be enabled on your trail to see Insights events in the console\. Allow up to 36 hours for CloudTrail to deliver the first Insights events, if unusual activity is detected\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/home/](https://console.aws.amazon.com/cloudtrail/home/)\.

1. In the navigation pane, choose **Dashboard** to see the five most recent Insights events, or **Insights** to see all Insights events logged in your account in the last 90 days\.

   On the **Insights** page, you can filter Insights events by criteria including event API source, event name, and event ID, and limit the events displayed to those occurring within a specific time range\. For more information about filtering Insights events, see [Filtering Insights Events](#filtering-insights-events)\.

**Contents**
+ [Filtering Insights Events](#filtering-insights-events)
+ [Viewing Details for an Insights Event](#viewing-details-for-an-event)
+ [Zoom, Pan, and Download Graph](#viewing-insight-details-zoom)
+ [Change Graph Time Span Settings](#viewing-insight-details-timespan)
+ [Downloading Insights Events](#downloading-insights-events)

## Filtering Insights Events<a name="filtering-insights-events"></a>

The default display of events in **Insights** shows events in reverse chronological order, with the newest Insights events, sorted by event start time, at the top\. The following list describes the available attributes\.

**Event name**  
The name of the event, typically the AWS API on which unusual levels of activity were recorded\.

**Event source**  
The AWS service to which the request was made, such as `iam.amazonaws.com` or `s3.amazonaws.com`\. You can scroll through a list of event sources after you choose the **Event source** filter\.

**Event ID**  
The ID of the Insights event\. Event IDs are not shown in the **Insights** page table, but they are an attribute on which you can filter Insights events\. The event IDs of management events that are analyzed to generate Insights events are different from the event IDs of Insights events\.

**Start time**  
The start time of the Insights event, measured as the first minute in which unusual API activity was recorded\.

If there are no events logged for the attribute or time that you choose, the results list is empty\. You can apply only one attribute filter in addition to the time range\. If you choose a different attribute filter, your specified time range is preserved\.

The following steps describe how to filter by attribute\.

**To filter by attribute**

1. To filter the results by an attribute, choose **Select attribute**, and then type or choose a value in the **Enter lookup value** box\.

1. To remove an attribute filter, choose the **X** on the right of the attribute filter box\.

The following steps describe how to filter by a start and end date and time\.

**To filter by a start and end date and time**

1. To narrow the time range for the events that you want to see, choose **Select time range**\.

1. In **From**, choose the day and specify the time that you want to be the start of the time range\.

1. In **To**, choose the day and specify the time that you want to be the end of the time range\. Choose **Apply**\.

1. To remove a time range filter, choose the calendar icon on the right of the **Time range** box, and then choose **Remove**\.

## Viewing Details for an Insights Event<a name="viewing-details-for-an-event"></a>

1. Choose an Insights event in the results list to show its details\. The details page for an Insights event shows a graph of the unusual activity timeline\.  
![\[A CloudTrail Insights detail page showing unusual API activity that was logged as an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_view.png)

1. Hover over the highlighted bands to show statistics about each Insights event in the graph\.  
![\[Insights event statistics shown after hovering over an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_statistics.png)

   The following information is shown when you hover over an Insights event in the graph:
   + Statistics
     + **Baseline average** \- The typical rate of calls per minute to this API in a specific region in your account\.
     + **Insights average** \- The rate of calls per minute to this API that triggered the Insights event\. The CloudTrail Insights average for the start event is the rate of calls per minute to the API that triggered the Insights event\. Typically, this is the first minute of unusual activity\. The Insights average for the end event is the rate of API calls per minute over the duration of the unusual activity, between the start Insights event and the end Insights event\.
   + Details about the Insights event
     + **Event start time** \- The minute during which unusual activity was first recorded\.
     + **Event end time** \- The minute during which unusual activity was last recorded\.

1. Expand **Insights event record** in the details pane to view the Insights event in JSON format\.

1. On the **CloudTrail events** tab, view related events that CloudTrail analyzed to determine that unusual activity occurred\. Note that a filter is already applied for the Insights event name, which is also the name of the related API\. The **CloudTrail events** tab shows CloudTrail management events related to the subject API that occurred between the start time \(minus one minute\) and end time \(plus one minute\) of the Insights event\.

   \. As you select other Insights events in the graph, the events shown in the **CloudTrail events** table change\. These events help you perform deeper analysis to determine the probable cause of an Insights event, and reasons for unusual API activity\.

1. In the left pane of the details page, choosing the linked **Event source** returns you to the **Insights** page, filtered by that event source\.

1. To remove the filter and view all CloudTrail events, not just the ones related to the Insights event subject API, choose **X** in the **Enter lookup value** box\.

## Zoom, Pan, and Download Graph<a name="viewing-insight-details-zoom"></a>

You can zoom, pan, and reset the axes of the graph on the Insights event details page by using a toolbar in the upper right corner\.

![\[Download as PNG, zoom, pan, zoom in, zoom out, and reset axes command toolbar.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_details_custom_widgets.png)

From left to right, the command buttons on the graph toolbar do the following:
+ **Download plot as a PNG** \- Download the graph image shown on the details page, and save it in PNG format\.
+ **Zoom** \- Drag to select an area on the graph that you want to enlarge and see in greater detail\.
+ **Pan** \- Shift the graph to see adjacent dates or times\.
+ **Reset axes** \- Change graph axes back to the original, clearing zoom and pan settings\.

## Change Graph Time Span Settings<a name="viewing-insight-details-timespan"></a>

You can change the time span—the selected duration of the events shown on the *x* axis—that is shown in the graph by choosing a setting in the graph's upper right corner\. The default time span that is shown in the graph depends on the duration of the selected Insights event\.


| Duration of Insights event | Default time span | 
| --- | --- | 
|  Less than 4 hours  |  **3h** \(three hours\)  | 
|  Between 4 and 12 hours  |  **12h**\(12 hours\)  | 
|  Between 12 and 24 hours  |  **1d** \(one day\)  | 
|  Between 24 and 72 hours  |  **3d** \(three days\)  | 
|  Longer than 72 hours  |  **1w** \(one week\)  | 

You can choose one hour, 12 hours, one day, three days, one week, or **custom**\. The following screenshot shows **Relative** time periods you can choose in **custom** settings\. Relative time periods are approximate time periods surrounding the start and end of the selected Insights event that is displayed on an Insights event details page\.

![\[Insights graph time span custom configuration, Relative time\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_graph_custom.png)

To specify an exact date and time range, choose the **Absolute** tab\.

![\[Insights graph time span custom configuration, Absolute time.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_details_custom_absolute.png)

## Downloading Insights Events<a name="downloading-insights-events"></a>

You can download recorded Insights event history as a file in CSV or JSON format\. Use filters and time ranges to reduce the size of the file you download\.

**Note**  
CloudTrail event history files are data files that contain information \(such as resource names\) that can be configured by individual users\. Some data can potentially be interpreted as commands in programs used to read and analyze this data \(CSV injection\)\. For example, when CloudTrail events are exported to CSV and imported to a spreadsheet program, that program might warn you about security concerns\. As a security best practice, disable links or macros from downloaded event history files\.

1. Specify the filter and time range for events you want to download\. For example, you can specify the event name, `StartInstances`, and specify a time range for the last three days of activity\.

1. Choose ![\[download icon\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/history-download.png) and then choose **Download CSV** or **Download JSON**\. The download starts immediately\.
**Note**  
Your download might take some time to complete\. For faster results, before you start the download process, use a more specific filter or a shorter time range to narrow the results\.

1. After your download is complete, open the file to view the events that you specified\.

1. To cancel your download, choose **Cancel download**\.