# Logging Insights events for trails<a name="logging-insights-events-with-cloudtrail"></a>

AWS CloudTrail Insights helps AWS users identify and respond to unusual activity associated with `write` API calls by continuously analyzing CloudTrail management events\.

Insights events are logged when CloudTrail detects unusual `write` management API activity in your account\. If you have CloudTrail Insights enabled and CloudTrail detects unusual activity, Insights events are delivered to the destination S3 bucket for your trail\. You can also see the type of insight and the incident time period when you view Insights events on the CloudTrail console\. Unlike other types of events captured in a CloudTrail trail, Insights events are logged only when CloudTrail detects changes in your account's API usage that differ significantly from the account's typical usage patterns\.

CloudTrail Insights continuously monitors CloudTrail `write` management events, and uses mathematical models to determine the normal levels of API event and error rate activity for an account\. CloudTrail Insights identifies behavior that is outside normal patterns, generates Insights events, and delivers those events to a `/CloudTrail-Insight` folder in the chosen destination S3 bucket for your trail\. You can also access and view Insights events in the AWS Management Console for CloudTrail\. For more information about how to access and view Insights events in the console and by using the AWS CLI, see [Viewing CloudTrail Insights events](view-insights-events.md) in this guide\.

By default, trails log all management events and don't include data events or Insights events\. Additional charges apply for data and Insights events\. For more information, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

When an event occurs in your account, CloudTrail evaluates whether the event matches the settings for your trails\. Only events that match your trail settings are delivered to your Amazon S3 bucket and Amazon CloudWatch Logs log group\.

**Contents**
+ [Understanding CloudTrail Insights](#insights-events-understanding)
  + [Filter column](#insights-understanding-leftcolumn)
  + [**Insights graph** tab](#insights-understanding-graph)
  + [**Attributions** tab](#insights-understanding-attributions)
    + [Baseline average and Insights average](#insights-understanding-average)
  + [**CloudTrail events** tab](#insights-understanding-ctevents)
  + [**Insights event record** tab](#insights-understanding-record)
+ [Logging Insights events with the AWS Management Console](#insights-events-enable)
+ [Logging Insights events with the AWS Command Line Interface](#insights-events-CLI-enable)
+ [Logging events with the AWS SDKs](#insights-events-logging-SDK)
+ [Sending events to Amazon CloudWatch Logs](#insights-events-logging-CWL)

## Understanding CloudTrail Insights<a name="insights-events-understanding"></a>

CloudTrail Insights can help you detect unusual API volume or error rate activity in your AWS account by raising Insights events\. CloudTrail Insights measures your normal patterns of API call volume and API error rates, also called the *baseline*, and generates Insights events when the call volume or error rates are outside normal patterns\. Insights events on API call volume are generated for `write` management APIs, and Insights events on API error rate are generated for both `read` and `write` management APIs\.

After you enable CloudTrail Insights for the first time on a trail, it can take up to 36 hours for CloudTrail to deliver the first Insights event, if unusual activity is detected\. CloudTrail Insights analyzes management events that occur in a single Region, not globally\. A CloudTrail Insights event is generated in the same Region as its supporting management events are generated\.

The following image shows an example of Insights events\. You open details pages for an Insights event by choosing an Insights event name from the **Dashboard** or **Insights** pages\.

If you disable CloudTrail Insights on a trail, or stop logging on a trail \(which disables CloudTrail Insights\), you may have Insights events stored in your destination S3 bucket, or shown on the **Insights** page of the console, that date from the earlier time that you had Insights enabled\.

### Filter column<a name="insights-understanding-leftcolumn"></a>

The left column lists Insights events that are related to the subject API, and that have the same Insights event type\. The column lets you choose the Insights event about which you want more information\. When you choose an event in this column, the event is highlighted in the graph on the **Insights graph** tab\. By default, CloudTrail applies a filter that limits events shown on the **CloudTrail events** tab to those about the specific API that was called during the period of unusual activity that triggered the Insights event\. To show all CloudTrail events called during the period of unusual activity, including events unrelated to the Insights event, turn off the filter\.

### **Insights graph** tab<a name="insights-understanding-graph"></a>

On the **Insights graph** tab, the details page for an Insights event shows a graph of an API's call volume or error rate that occurred over a period of time before and after one or more Insights events are logged\. In the graph, Insights events are highlighted with vertical bars, with the width of the bar showing the start and end time of the Insights event\.

In this example, a vertical highlighting band shows unusual numbers of AWS Systems Manager `SendCommand` API calls in an account\. In the highlighted area, because the number of `SendCommand` calls rose above the account's baseline average of 0\.0442 calls per minute, CloudTrail logged an Insights event when it detected the unusual activity\. The Insights event recorded that as many as 15 `SendCommand` calls were made in a five\-minute period between 5:50 and 5:55 a\.m\. This is about two more calls to that API per minute than is expected for the account\. In this example, the graph's time span is three hours: 4:30 a\.m\. PDT on July 15, 2021 to 7:30 a\.m\. PDT on July 15, 2021\. This event has a start time of 6:00 a\.m\. PDT on July 15, 2021, and an end time two minutes later\. An ending Insights event, not highlighted, shows that the unusual activity ended at about 6:16 a\.m\.

The baseline is calculated over the seven days preceding the start of an Insights event\. Though the value of the baseline duration—the period that CloudTrail measures for normal activity on APIs—is approximately seven days, CloudTrail rounds the baseline duration to a whole integer day, so the exact baseline duration can vary\.

![\[A CloudTrail Insights detail page showing unusual API activity that was logged as an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_callrate_view.png)

You can use the **Zoom** command on the toolbar to zoom in on the ending Insights event, showing the start and end time\. In this example, choosing **Zoom**, then dragging the **Zoom** cursor a very short distance over one edge of the highlighted Insights event expands the Insights event and shows more timeline detail\.

![\[A CloudTrail Insights event, zoomed in to show timeline detail.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_zoom.png)

To view CloudTrail events that were analyzed to determine unusual activity, open the **CloudTrail events** tab\. In this example, CloudTrail analyzed 12 events, four of which triggered the Insights event\.

![\[A CloudTrail Insights event, zoomed in to show timeline detail.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_related_events.png)

The following screenshot shows an Insights graph tab for an API error rate Insights event\. The highlighted area shows that an Insights event was logged because occurrences of the `NoSuchEntityException` error on the `GetRolePolicy` IAM API call rose above the baseline average of 0\.0017 `NoSuchEntityException` errors per minute on this API call, averaging 18 errors per minute during the insight period\. The number of CloudTrail events that triggered the Insights event matches the Insights average of 18 `NoSuchEntityException` errors in one minute, in this example\. Unlike an API call rate graph, the API error rate shows two lines, in contrasting colors: a line measuring calls to the IAM API, `GetRolePolicy`, that resulted in an unusual number of errors, and a line measuring the error on which unusual activity was logged, `NoSuchEntityException`\.

![\[A CloudTrail Insights detail page showing unusual error rate activity that was logged as an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_errorrate.png)

### **Attributions** tab<a name="insights-understanding-attributions"></a>

The **Attributions** tab shows the following information about an Insights event\. Information on the **Attributions** tab can help you identify the causes and sources of Insights activity\. Expand the top baseline areas to compare user identity, user agent, and error code activity during normal periods with those attributed during the Insights activity\. In **Top baseline user identity ARNs**, **Top baseline user agents**, and **Top baseline error codes**, only the *baseline average*—the historic average of events for the API that are logged by the user identity, user agent, or that result in the error code, in approximately the seven days before the Insights event start time—is shown\.

![\[A CloudTrail Insights event detail page showing attributions.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_attributions.png)

The **Attributions** tab shows only top user identity ARNs and top user agents for an error rate Insights event, as shown in the following image\. Top error codes are not necessary for error rate Insights events\.

![\[A CloudTrail Insights event detail page showing attributions for an error rate Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_attributions_errorrate.png)
+ **Top user identity ARNs** \- This table shows up to the top ﬁve AWS users or IAM roles \(user identities\) that contributed to API calls during the unusual activity and baseline periods, in descending order by the average number of API calls contributed\. The percentage of the averages as a total of activity that contributed to the unusual activity is shown in parentheses\. If more than five user identity ARNs contributed to the unusual activity, their activity is summed up in an **Other** row\.
+ **Top user agents** \- This table shows up to the top ﬁve AWS tools by which the user identity contributed to API calls during the unusual activity and baseline periods, in descending order by the average number of API calls contributed\. These tools include the AWS Management Console, AWS CLI, or the AWS SDKs\. For example, a user agent named `ec2.amazonaws.com` indicates that the Amazon EC2 console was among the tools used to call the API\. The percentage of the averages as a total of activity that contributed to the unusual activity is shown in parentheses\. If more than five user agents contributed to the unusual activity, their activity is summed up in an **Other** row\.
+ **Top error codes** \- Only shown for **API call rate** Insights events\. This table shows up to the top five error codes that occurred on API calls during the unusual activity and baseline periods, in descending order from largest number of API calls to smallest\. The percentage of the averages as a total of activity that contributed to the unusual activity is shown in parentheses\. If more than five error codes occurred during the unusual or baseline activity, their activity is summed up in an **Other** row\.

  A value of `None` as one of the top five error code values means that a significant percentage of the calls that contributed to the Insights event did not result in errors\. If the error code value is `None`, and there are no other error codes in the table, the values in the **Insight average** and **Baseline average** columns are the same as those for the Insights event overall\. You can also see those values displayed in the **Insight average** and **Baseline average** legend on the **Insights graph** tab, under **API calls per minute**\.

#### Baseline average and Insights average<a name="insights-understanding-average"></a>

**Baseline average** and **Insights average** are shown for top user identities, top user agents, and top error codes\.
+ **Baseline average** \- The typical rate of occurrences per minute on the API on which the Insights event was logged, as measured within approximately the preceding seven days, in a specific Region in your account\.
+ **Insights average** \- The rate of calls to or errors on this API that triggered the Insights event\. The CloudTrail Insights average for the start event is the rate of calls or errors per minute on the API that triggered the Insights event\. Typically, this is the first minute of unusual activity\. The Insights average for the end event is the rate of API calls or errors per minute over the duration of the unusual activity, between the start Insights event and the end Insights event\.

### **CloudTrail events** tab<a name="insights-understanding-ctevents"></a>

On the **CloudTrail events** tab, view related events that CloudTrail analyzed to determine that unusual activity occurred\. By default, a filter is already applied for the Insights event name, which is also the name of the related API\. To show all CloudTrail events logged during the period of unusual activity, turn off **Only show events for selected Insights event**\. The **CloudTrail events** tab shows CloudTrail management events related to the subject API that occurred between the start and end time of the Insights event\. These events help you perform deeper analysis to determine the probable cause of an Insights event, and reasons for unusual API and error rate activity\.

### **Insights event record** tab<a name="insights-understanding-record"></a>

Like any CloudTrail event, a CloudTrail Insights event is a record in JSON format\. The **Insights event record** tab shows the JSON structure and content of the Insights start and end events, sometimes called the event *payload*\. For more information about the fields and content of the Insights event record, see [Record fields for Insights events](cloudtrail-event-reference-record-contents.md#eventrecord-insight) and [CloudTrail Insights `insightDetails` element](cloudtrail-event-reference-insight-details.md) in this guide\.

## Logging Insights events with the AWS Management Console<a name="insights-events-enable"></a>

Enable CloudTrail Insights events on an existing trail\. By default, Insights events are not enabled\.

1. In the left navigation pane of the CloudTrail console, open the **Trails** page, and choose a trail name\.

1. In **Insights events** choose **Edit**\.
**Note**  
Additional charges apply for logging Insights events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

1. In **Event type**, choose **Insights events**\. You must be logging **Write** management events to log Insights events\.

1. In **Insights events**, under **Choose Insights types**, choose **API call rate**, **API error rate**, or both\.

1. Choose **Save changes** to save your changes\.

It can take up to 36 hours for CloudTrail to deliver the first Insights events, if unusual activity is detected\.

## Logging Insights events with the AWS Command Line Interface<a name="insights-events-CLI-enable"></a>

You can configure your trails to log Insights events using the AWS CLI\.

To view whether your trail is logging Insights events, run the `get-insight-selectors` command\.

```
aws cloudtrail get-insight-selectors --trail-name TrailName
```

The following result shows the default settings for a trail\. By default, trails don't log Insights events\. The `InsightType` attribute value is empty, and no Insight event selectors are specified, because Insights event collection is not enabled\. 

If you do not add Insights selectors, the get\-insight\-selectors command returns the following error message: "An error occurred \(InsightNotEnabledException\) when calling the GetInsightSelectors operation: Trail *name* does not have Insights enabled\. Edit the trail settings to enable Insights, and then try the operation again\."

```
{
  "InsightSelectors": [ ],
  "TrailARN": "arn:aws:cloudtrail:us-east-1:123456789012:trail/TrailName"
}
```

To configure your trail to log Insights events, run the `put-insight-selectors` command\. The following example shows how to configure your trail to include Insights events\. Insights selector values can be `ApiCallRateInsight`, `ApiErrorRateInsight`, or both\.

```
aws cloudtrail put-insight-selectors --trail-name TrailName --insight-selectors '[{"InsightType": "ApiCallRateInsight"},{"InsightType": "ApiErrorRateInsight"}]'
```

The following result shows the Insights event selector that is configured for the trail\.

```
{
  "InsightSelectors":
      [
         {
            "InsightType": "ApiErrorRateInsight"
         },
         {
            "InsightType": "ApiCallRateInsight"
         }
      ],
  "TrailARN": "arn:aws:cloudtrail:us-east-1:123456789012:trail/TrailName"
}
```

## Logging events with the AWS SDKs<a name="insights-events-logging-SDK"></a>

Run the [GetInsightSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_GetInsightSelectors.html) operation to see whether your trail is logging Insights events for a trail\. You can configure your trails to log Insights events with the [PutInsightSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutInsightSelectors.html) operation\. For more information, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

## Sending events to Amazon CloudWatch Logs<a name="insights-events-logging-CWL"></a>

CloudTrail supports sending Insights events to CloudWatch Logs\. When you configure your trail to send Insights events to your CloudWatch Logs log group, CloudTrail Insights sends only the events that you specify in your trail\. For example, if you configure your trail to log management and Insights events, your trail delivers management and Insights events to your CloudWatch Logs log group\. To configure CloudWatch Events with the CloudWatch console or API, choose the `AWS Insight via CloudTrail` event type on the **Create rule** page in the CloudWatch console\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

The following images show an example rule, **Insights\-test\-rule**, created in CloudWatch Events\. When CloudTrail logs Insights events, the rule targets an Amazon SNS topic to send notifications to recipients who are specified in the SNS topic\.

![\[Configuring a new CloudWatch Events rule for CloudTrail Insights.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_cloudwatch_event_rule_setup.png)

After you choose the event source and targets, name the rule and provide a description\. When you're finished, choose **Create rule**\.

![\[Name a new CloudWatch Events rule for CloudTrail Insights.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_cloudwatch_event_rule_name.png)

When CloudTrail logs Insights events, recipients of the SNS topic should receive SNS notifications\.