# Logging Insights Events for Trails<a name="logging-insights-events-with-cloudtrail"></a>

AWS CloudTrail Insights helps AWS users identify and respond to unusual activity associated with `write` API calls by continuously analyzing CloudTrail management events\.

Insights events are logged when CloudTrail detects unusual `write` management API activity in your account\. If you have CloudTrail Insights enabled, and CloudTrail detects unusual activity, Insights events are delivered to the destination S3 bucket for your trail\. You can also see the type of insight and the incident time period when you view Insights events on the CloudTrail console\. Unlike other types of events captured in a CloudTrail trail, Insights events are logged only when CloudTrail detects changes in your account's API usage that differ significantly from the account's typical usage patterns\.

CloudTrail Insights continuously monitors CloudTrail `write` management events, and uses mathematical models to determine the normal levels of API and service event activity for an account\. CloudTrail Insights identifies behavior that is outside normal patterns, generates Insights events, and delivers those events to a `/CloudTrail-Insight` folder in the chosen destination S3 bucket for your trail\. You can also access and view Insights events in the AWS Management Console for CloudTrail\. For more information about how to access and view Insights events in the console and by using the AWS CLI, see [Viewing CloudTrail Insights Events](view-insights-events.md) in this guide\.

By default, trails log all management events and don't include data events or Insights events\. Additional charges apply for data and Insights events\. For more information, see [ AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

When an event occurs in your account, CloudTrail evaluates whether the event matches the settings for your trails\. Only events that match your trail settings are delivered to your Amazon S3 bucket and Amazon CloudWatch Logs log group\.

**Contents**
+ [Understanding Insights](#insights-events-understanding)
  + [Filter column](#insights-understanding-leftcolumn)
  + [**Insights graph** tab](#insights-understanding-graph)
  + [**Attributions** tab](#insights-understanding-attributions)
  + [**CloudTrail events** tab](#insights-understanding-ctevents)
  + [**Insights event record** tab](#insights-understanding-record)
+ [Logging Insights events with the AWS Management Console](#insights-events-enable)
+ [Logging Insights Events with the AWS Command Line Interface](#insights-events-CLI-enable)
+ [Logging Events with the AWS SDKs](#insights-events-logging-SDK)
+ [Sending Events to Amazon CloudWatch Logs](#insights-events-logging-CWL)

## Understanding Insights<a name="insights-events-understanding"></a>

CloudTrail Insights can help you detect unusual API activity in your AWS account by raising Insights events\. CloudTrail Insights measures your normal patterns of API call volume, also called the *baseline*, and generates Insights events when the volume is outside normal patterns\. Insights events are generated for `write` management APIs\.

After you enable CloudTrail Insights for the first time on a trail, it can take up to 36 hours for CloudTrail to deliver the first Insights event, if unusual activity is detected\. CloudTrail Insights analyzes write management events that occur in a single Region, not globally\. A CloudTrail Insights event is generated in the same Region as its supporting management events are generated\.

The following image shows an example of Insights events\. You open details pages for an Insights event by choosing an Insights event name from the **Dashboard** or **Insights** pages\.

If you disable CloudTrail Insights on a trail, or stop logging on a trail \(which disables CloudTrail Insights\), you may have Insights events stored in your destination S3 bucket, or shown on the **Insights** page of the console, that date from the earlier time that you had Insights enabled\.

### Filter column<a name="insights-understanding-leftcolumn"></a>



The left column lists Insights events that are related to the subject API, and that have the same Insights event type\. The column lets you choose the Insights event about which you want more information\. When you choose an event in this column, the event is highlighted in the graph on the **Insights graph** tab\. By default, CloudTrail applies a filter that limits events shown on the **CloudTrail events** tab to those about the specific API that was called during the period of unusual activity that triggered the Insights event\. To show all CloudTrail events called during the period of unusual activity, including events unrelated to the Insights event, turn off the filter\.

### **Insights graph** tab<a name="insights-understanding-graph"></a>

On the **Insights graph** tab, the details page for an Insights event shows a graph of an API's call volume that occurred over a period of time before and after one or more Insights events are logged\. In the graph, Insights events are highlighted with vertical bars, with the width of the bar showing the start and end time of the Insights event\.

In this example, a vertical highlighting band shows unusual numbers of Amazon CloudWatch `DeleteAlarms` API calls in an account\. In the highlighted area, because the number of `DeleteAlarms` calls rose above the account's baseline average of 0\.05 calls per minute, CloudTrail logged an Insights event when it detected the unusual activity\. The Insights event recorded that as many as 3 `DeleteAlarms` calls were made at about 11:20 a\.m\. This is about three more calls to that API per minute than is expected for the account\. In this example, the graph's time span is three hours: 9:50 a\.m\. PDT on August 5, 2020 to 12:50 p\.m\. PDT on August 5, 2020\. This event has a start time of 11:20 a\.m\. PDT on August 5, 2020, and an end time one minute later\.

The baseline is calculated over the seven days preceding the start of an Insights event\. Though the value of the baseline duration—the period that CloudTrail measures for normal activity on APIs—is approximately seven days, CloudTrail rounds the baseline duration to a whole integer day, so the exact baseline duration can vary\.

![\[A CloudTrail Insights detail page showing unusual API activity that was logged as an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_view.png)

### **Attributions** tab<a name="insights-understanding-attributions"></a>

The **Attributions** tab shows the following information about an Insights event\.

![\[A CloudTrail Insights event detail page showing attributions.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_attributions.png)
+ **Top user identity ARNs** \- This table shows up to the top ﬁve AWS users or IAM roles \(user identities\) that contributed to API calls during the unusual activity and baseline periods, in descending order by the average number of API calls contributed\. The percentage of the averages as a total of activity that contributed to the unusual activity is shown in parentheses\. If more than five user identity ARNs contributed to the unusual activity, their activity is summed up in an **Other** row\.
+ **Top user agents** \- This table shows up to the top ﬁve AWS tools by which the user identity contributed to API calls during the unusual activity and baseline periods, in descending order by the average number of API calls contributed\. These tools include the AWS Management Console, AWS CLI, or the AWS SDKs\. For example, a user agent named `ec2.amazonaws.com` indicates that the Amazon EC2 console was among the tools used to call the API\. The percentage of the averages as a total of activity that contributed to the unusual activity is shown in parentheses\. If more than five user agents contributed to the unusual activity, their activity is summed up in an **Other** row\.
+ **Top error codes** \- This table shows up to the top five error codes that occurred on API calls during the unusual activity and baseline periods, in descending order from largest number of API calls to smallest\. The percentage of the averages as a total of activity that contributed to the unusual activity is shown in parentheses\. If more than five error codes occurred during the unusual or baseline activity, their activity is summed up in an **Other** row\.

  A value of `None` as one of the top five error code values means that a significant percentage of the calls that contributed to the Insights event did not result in errors\. If the error code value is `None`, and there are no other error codes in the table, the values in the **Insight average** and **Baseline average** columns are the same as those for the Insights event overall\. You can also see those values displayed in the **Insight average** and **Baseline average** legend on the **Insights graph** tab, under **API calls per minute**\.

### **CloudTrail events** tab<a name="insights-understanding-ctevents"></a>

On the **CloudTrail events** tab, view related events that CloudTrail analyzed to determine that unusual activity occurred\. By default, a filter is already applied for the Insights event name, which is also the name of the related API\. To show all CloudTrail events logged during the period of unusual activity, turn off **Only show events for selected Insights event**\. The **CloudTrail events** tab shows CloudTrail management events related to the subject API that occurred between the start and end time of the Insights event\. These events help you perform deeper analysis to determine the probable cause of an Insights event, and reasons for unusual API activity\.

### **Insights event record** tab<a name="insights-understanding-record"></a>

Like any CloudTrail event, a CloudTrail Insights event is a record in JSON format\. The **Insights event record** tab shows the JSON structure and content of the Insights start and end events, sometimes called the event *payload*\. For more information about the fields and content of the Insights event record, see [Insights Event Record Fields](cloudtrail-event-reference-record-contents.md#eventrecord-insight) and [CloudTrail Insights `insightDetails` element](cloudtrail-event-reference-insight-details.md) in this guide\.

## Logging Insights events with the AWS Management Console<a name="insights-events-enable"></a>

Enable CloudTrail Insights events on an existing trail\. By default, Insights events are not enabled\.

1. In the left navigation pane of the CloudTrail console, open the **Trails** page, and choose a trail name\.

1. In **Insights events** choose **Edit**\.
**Note**  
Additional charges apply for logging Insights events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

1. In **Event type**, select **Insights events**\. You must be logging **Write** management events to log Insights events\.

1. Choose **Update trail** to save your changes\.

It can take up to 36 hours for CloudTrail to deliver the first Insights events, if unusual activity is detected\.

## Logging Insights Events with the AWS Command Line Interface<a name="insights-events-CLI-enable"></a>

You can configure your trails to log Insights events using the AWS CLI\.

To view whether your trail is logging Insights events, run the `get-insight-selectors` command\.

```
aws cloudtrail get-insight-selectors --trail-name TrailName
```

The following result shows the default settings for a trail\. By default, trails don't log Insights events\. The `InsightType` attribute value is empty, and no Insight event selectors are specified, because Insights event collection is not enabled\.

```
{
  "InsightSelectors": 
    [
      { 
        "InsightType": "" 
      }
    ], 
  "TrailARN": "arn:aws:cloudtrail:us-east-1:123456789012:trail/TrailName"
}
```

To configure your trail to log Insights events, run the `put-insight-selectors` command\. The following example shows how to configure your trail to include Insights events\. In this release, the only Insights selector is `ApiCallRateInsight`\.

```
aws cloudtrail put-insight-selectors --trail-name TrailName --insight-selectors '[{"InsightType": "ApiCallRateInsight"}]'
```

The following result shows the Insights event selector that is configured for the trail\.

```
{
  "InsightSelectors": 
    [
      {
        "InsightType": "ApiCallRateInsight"
      }
    ],
  "TrailARN": "arn:aws:cloudtrail:us-east-1:123456789012:trail/TrailName"
}
```

## Logging Events with the AWS SDKs<a name="insights-events-logging-SDK"></a>

Run the [GetInsightSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_GetInsightSelectors.html) operation to see whether your trail is logging Insights events for a trail\. You can configure your trails to log Insights events with the [PutInsightSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutInsightSelectors.html) operation\. For more information, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

## Sending Events to Amazon CloudWatch Logs<a name="insights-events-logging-CWL"></a>

CloudTrail supports sending Insights events to CloudWatch Logs\. When you configure your trail to send Insights events to your CloudWatch Logs log group, CloudTrail Insights sends only the events that you specify in your trail\. For example, if you configure your trail to log management and Insights events, your trail delivers management and Insights events to your CloudWatch Logs log group\. To configure CloudWatch Events with the CloudWatch console or API, choose the `AWS Insight via CloudTrail` event type on the **Create rule** page in the CloudWatch console\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.