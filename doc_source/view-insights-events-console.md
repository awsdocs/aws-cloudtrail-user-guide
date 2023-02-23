# Viewing CloudTrail Insights events in the CloudTrail console<a name="view-insights-events-console"></a>

After you enable CloudTrail Insights events on a trail, when CloudTrail detects unusual API or error rate activity, CloudTrail generates Insights events and displays them on the **Dashboard** and **Insights** pages in the AWS Management Console\. You can view the Insights events in the console and troubleshoot the unusual activity\. The most recent 90 days of Insights events are shown in the console\. You can also download Insights events by using the AWS CloudTrail console\. You can programmatically look up events by using the AWS SDKs or AWS Command Line Interface\. For more information about CloudTrail Insights events, see [Logging Insights events for trails](logging-insights-events-with-cloudtrail.md) in this guide\.

**Note**  
 To log Insights events on API call volume, the trail must log `write` management events\. To log Insights events on API error rate, the trail must log `read` or `write` management events\. 

After Insights events are logged, the events are shown on the **Insights** page for 90 days\. You cannot manually delete events from the **Insights** page\. Because you must [create a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) before you can enable CloudTrail Insights, you can view Insights events that are logged to your trail for as long as you store them in the S3 bucket that is configured in your trail settings\.

Monitor your trail logs and be notified when specific Insights events activity occurs with Amazon CloudWatch Logs\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

**To view Insights events**

CloudTrail Insights events must be enabled on your trail to see Insights events in the console\. Allow up to 36 hours for CloudTrail to deliver the first Insights events, if unusual activity is detected\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/home/](https://console.aws.amazon.com/cloudtrail/home/)\.

1. In the navigation pane, choose **Dashboard** to see the five most recent Insights events, or **Insights** to see all Insights events logged in your account in the last 90 days\.

   On the **Insights** page, you can filter Insights events by criteria including event API source, event name, and event ID, and limit the events displayed to those occurring within a specific time range\. For more information about filtering Insights events, see [Filtering Insights events](#filtering-insights-events)\.

**Contents**
+ [Filtering Insights events](#filtering-insights-events)
+ [Viewing Insights events details](#viewing-details-for-an-event)
+ [Zoom, pan, and download graph](#viewing-insight-details-zoom)
+ [Change graph time span settings](#viewing-insight-details-timespan)
+ [Downloading Insights events](#downloading-insights-events)

## Filtering Insights events<a name="filtering-insights-events"></a>

The default display of events in **Insights** shows events in reverse chronological order\. The newest Insights events, sorted by event start time, are at the top\. The following list describes the available attributes\. You can filter on the first three attributes: **Event name**, **Event source**, and **Event ID**\.

![\[The CloudTrail Insights event list filter.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_events_filter.png)

**Event name**  
The name of the event, typically the AWS API on which unusual levels of activity were recorded\.

**Insight type**  
The type of CloudTrail Insights event, which is either **API call rate** or **API error rate**\. The **API call rate** insight type analyzes write\-only management API calls that are aggregated per minute against a baseline API call volume\. The **API error rate** insight type analyzes management API calls that result in error codes\. The error is shown if the API call is unsuccessful\.

**Event source**  
The AWS service to which the request was made, such as `iam.amazonaws.com` or `s3.amazonaws.com`\. You can scroll through a list of event sources after you choose the **Event source** filter\.

**Event ID**  
The ID of the Insights event\. Event IDs are not shown in the **Insights** page table, but they are an attribute on which you can filter Insights events\. The event IDs of management events that are analyzed to generate Insights events are different from the event IDs of Insights events\.

**Event start time**  
The start time of the Insights event, measured as the first minute in which unusual activity was recorded\. This attribute is shown in the **Insights** table, but you cannot filter on event start time in the console\.

**Baseline average**  
The normal pattern of API call rate or error rate activity\. The baseline average is calculated over the seven days preceding the start of an Insights event\. Though the value of the baseline duration—the period that CloudTrail analyzes for normal activity on APIs—is approximately seven days, CloudTrail rounds the baseline duration to a whole integer day, so the exact baseline duration can vary\.

**Insight average**  
The average number of calls to an API, or the average number of a specific error that was returned on calls to an API, that triggered the Insights event\. The CloudTrail Insights average for the start event is the rate of occurrences that triggered the Insights event\. Typically, this is the first minute of unusual activity\. The Insights average for the end event is the rate of occurrences over the duration of the unusual activity, between the start Insights event and the end Insights event\.

**Rate change**  
The difference between the value of **Baseline average** and **Insight average**, measured as a percentage\. For example, if the baseline average of an `AccessDenied` error occurring is 1\.0, and the Insight average is 3\.0, the rate change is 300%\. A rate change for an Insight average that exceeds a baseline average shows an up\-arrow next to the value\. If the Insights event was logged because the activity is below the baseline average, **Rate change** shows a down\-arrow next to the percentage\.

If there are no events logged for the attribute or time that you choose, the results list is empty\. You can apply only one attribute filter in addition to the time range\. If you choose a different attribute filter, your specified time range is preserved\.

The following steps describe how to filter by attribute\.

**To filter by attribute**

1. To filter the results by an attribute, choose a lookup attribute from the drop\-down menu, and then type or choose a value in the **Enter a lookup value** box\.

1. To remove an attribute filter, choose the **X** on the right of the attribute filter box\.

The following steps describe how to filter by a start and end date and time\.

**To filter by a start and end date and time**

1. To narrow the time range for the events that you want to see, choose a time range on the time span bar at the top of the table\. Preset time ranges include 30 minutes, 1 hour, 3 hours, or 12 hours\. To specify a custom time range, choose **Custom**\.

1. Choose one of the following tabs\.
   + **Absolute** \- Lets you choose a specific time\. Go on to the next step\.
   + **Relative to selected event** \- Selected by default\. Lets you choose a time period relative to the start time of an Insights event\. Go on to step 4\.

1. To set an **Absolute** time range, do the following\.

   1. On the **Absolute** tab, choose the day that you want the time range to start\. Enter a start time on the selected day\. To enter a date manually, type the date in the format `yyyy/mm/dd`\. The start and end times use a 24\-hour clock, and values must be in the format `hh:mm:ss`\. For example, to indicate a 6:30 p\.m\. start time, enter **18:30:00**\.

   1. Choose an end date for the range on the calendar, or specify an end date and time below the calendar\. Choose **Apply**\.

1. To set a **Relative to selected event** time range, do the following\.

   1. Choose a preset time period relative to the start time of Insights events\. Preset values are available in minutes, hours, days, or weeks\. The maximum relative time period is 12 weeks\.

   1. If needed, customize the preset value in the boxes below the presets\. Choose **Clear** to reset your changes if needed\. When you have set the relative time that you want, choose **Apply**\.

1. In **To**, choose the day and specify the time that you want to be the end of the time range\. Choose **Apply**\.

1. To remove a time range filter, choose the calendar icon on the right of the **Time range** box, and then choose **Remove**\.

## Viewing Insights events details<a name="viewing-details-for-an-event"></a>

1. Choose an Insights event in the results list to show its details\. The details page for an Insights event shows a graph of the unusual activity timeline\.  
![\[A CloudTrail Insights detail page showing unusual API activity that was logged as an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_view.png)

1. Hover over the highlighted bands to show the start time and duration of each Insights event in the graph\.  
![\[Insights event statistics shown after hovering over an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_statistics.png)

   The following information is shown in the **Additional information** area of the graph:
   + **Insight type**\. This can be API call rate or API error rate\.
   + **Trigger**\. This is a link to the **Cloudtrail events** tab, which lists the management events that were analyzed to determine that unusual activity occurred\.
   + **API calls per minute**
     + **Baseline average** \- The typical rate of occurrences per minute on the API on which the Insights event was logged, as measured within approximately the preceding seven days, in a specific Region in your account\.
     + **Insights average** \- The rate of occurrences per minute on this API that triggered the Insights event\. The CloudTrail Insights average for the start event is the rate of calls or errors per minute on the API that triggered the Insights event\. Typically, this is the first minute of unusual activity\. The Insights average for the end event is the rate of API calls or errors per minute over the duration of the unusual activity, between the start Insights event and the end Insights event\.
   + **Event source**\. The AWS service endpoint on which the unusual number of API calls or errors were logged\. In the preceding image, the source is `ec2.amazonaws.com`, which is the service endpoint for Amazon EC2\.
   + **Event IDs**\.
     + **Start event ID** \- The ID of the Insights event that was logged at the start of unusual activity\.
     + **End event ID** \- The ID of the Insights event that was logged at the end of unusual activity\.
     + **Shared event ID** \- In Insights events, the **Shared event ID** is a GUID that is generated by CloudTrail Insights to uniquely identify a start and end pair of Insights events\. **Shared event ID** is common between the start and the end Insights event, and helps to create a correlation between both events to uniquely identify unusual activity\.

1. Choose the **Attributions** tab to view information about the user identities, user agents, and on API call rate Insights events, error codes correlated with unusual and baseline activity\. A maximum of five user identities, five user agents, and five error codes are shown in tables on the **Attributions** tab, sorted by an average of the count of activity, in descending order from highest to lowest\. For more information about the **Attributions** tab, see [**Attributions** tab](logging-insights-events-with-cloudtrail.md#insights-understanding-attributions) and [CloudTrail Insights `insightDetails` element](cloudtrail-event-reference-insight-details.md) in this guide\.

1. On the **CloudTrail events** tab, view related events that CloudTrail analyzed to determine that unusual activity occurred\. By default, a filter is already applied for the Insights event name, which is also the name of the related API\. The **CloudTrail events** tab shows CloudTrail management events related to the subject API that occurred between the start time \(minus one minute\) and end time \(plus one minute\) of the Insights event\.

   As you select other Insights events in the graph, the events shown in the **CloudTrail events** table change\. These events help you perform deeper analysis to determine the probable cause of an Insights event and reasons for unusual API activity\.

   To show all CloudTrail events that were logged during the Insights event duration, and not only those for the related API, turn off the filter\.

1. Choose the **Insights event record** tab to view the Insights start and end events in JSON format\.

1. Choosing the linked **Event source** returns you to the **Insights** page, filtered by that event source\.

## Zoom, pan, and download graph<a name="viewing-insight-details-zoom"></a>

You can zoom, pan, and reset the axes of the graph on the Insights event details page by using a toolbar in the upper right corner\.

![\[Download as PNG, zoom, pan, zoom in, zoom out, and reset axes command toolbar.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_details_custom_widgets.png)

From left to right, the command buttons on the graph toolbar do the following:
+ **Download plot as a PNG** \- Download the graph image shown on the details page, and save it in PNG format\.
+ **Zoom** \- Drag to select an area on the graph that you want to enlarge and see in greater detail\.
+ **Pan** \- Shift the graph to see adjacent dates or times\.
+ **Reset axes** \- Change graph axes back to the original, clearing zoom and pan settings\.

## Change graph time span settings<a name="viewing-insight-details-timespan"></a>

You can change the time span—the selected duration of the events shown on the *x* axis—that is shown in the graph by choosing a setting in the graph's upper right corner\.

![\[Time span control for an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_details_timespan.png)

The default time span that is shown in the graph depends on the duration of the selected Insights event\.


| Duration of Insights event | Default time span | 
| --- | --- | 
|  Less than 4 hours  |  **3h** \(three hours\)  | 
|  Between 4 and 12 hours  |  **12h**\(12 hours\)  | 
|  Between 12 and 24 hours  |  **1d** \(one day\)  | 
|  Between 24 and 72 hours  |  **3d** \(three days\)  | 
|  Longer than 72 hours  |  **1w** \(one week\)  | 

You can choose presets of five minutes, 30 minutes, one hour, three hours, 12 hours, or **Custom**\. The following image shows **Relative to selected event** time periods you can choose in **Custom** settings\. Relative time periods are approximate time periods surrounding the start and end of the selected Insights event that is displayed on an Insights event details page\.

![\[Insights graph time span custom configuration, Relative time\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_details_custom_relative.png)

To customize a selected preset, specify a number and time unit in the boxes below the presets\.

To specify an exact date and time range, choose the **Absolute** tab\. If you set an absolute date and time range, start and end times are required\. For information about how to set the time, see [Filtering Insights events](#filtering-insights-events) in this topic\.

![\[Insights graph time span custom configuration, Absolute time.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_details_custom_absolute.png)

## Downloading Insights events<a name="downloading-insights-events"></a>

You can download recorded Insights event history as a file in CSV or JSON format\. Use filters and time ranges to reduce the size of the file you download\.

**Note**  
CloudTrail event history files are data files that contain information \(such as resource names\) that can be configured by individual users\. Some data can potentially be interpreted as commands in programs used to read and analyze this data \(CSV injection\)\. For example, when CloudTrail events are exported to CSV and imported to a spreadsheet program, that program might warn you about security concerns\. As a security best practice, disable links or macros from downloaded event history files\.

1. Specify the filter and time range for events you want to download\. For example, you can specify the event name, `StartInstances`, and specify a time range for the last three days of activity\.

1. Choose **Download events**, and then choose **Download CSV** or **Download JSON**\. You are prompted to choose a location to save the file\.
**Note**  
Your download might take some time to finish\. For faster results, before you start the download process, use a more specific filter or a shorter time range to narrow the results\.

1. After your download is complete, open the file to view the events that you specified\.

1. To cancel your download, choose **Cancel download**\. If you cancel a download before it is finished, a CSV or JSON file on your local computer might contain only part of your events\.