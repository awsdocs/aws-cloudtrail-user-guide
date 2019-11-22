# Logging Insights Events for Trails<a name="logging-insights-events-with-cloudtrail"></a>

AWS CloudTrail Insights helps AWS users identify and respond to unusual activity associated with `write` API calls by continuously analyzing CloudTrail management events\.

Insights events are logged when CloudTrail detects unusual `write` management API activity in your account\. If you have CloudTrail Insights enabled, and CloudTrail detects unusual activity, Insights events are delivered to the destination S3 bucket for your trail\. You can also see the type of insight and the incident time period when you view Insights events on the CloudTrail console\. Unlike other types of events captured in a CloudTrail trail, Insights events are logged only when CloudTrail detects changes in your account's API usage that differ significantly from the account's typical usage patterns\.

CloudTrail Insights continuously monitors CloudTrail `write` management events, and uses mathematical models to determine the normal levels of API and service event activity for an account\. CloudTrail Insights identifies behavior that is outside normal patterns, generates Insights events, and delivers those events to a `/CloudTrail-Insight` folder in the chosen destination S3 bucket for your trail\. You can also access and view Insights events in the AWS Management Console for CloudTrail\. For more information about how to access and view Insights events in the console and by using the AWS CLI, see [Viewing CloudTrail Insights Events](view-insights-events.md) in this guide\.

By default, trails log all management events and don't include data events or Insights events\. Additional charges apply for data and Insights events\. For more information, see [ AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

When an event occurs in your account, CloudTrail evaluates whether the event matches the settings for your trails\. Only events that match your trail settings are delivered to your Amazon S3 bucket and Amazon CloudWatch Logs log group\.

**Contents**
+ [Understanding Insights](#insights-events-understanding)
+ [Logging Insights Events with the AWS Management Console](#insights-events-enable)
+ [Logging Insights Events with the AWS Command Line Interface](#insights-events-CLI-enable)
+ [Logging Events with the AWS SDKs](#insights-events-logging-SDK)
+ [Sending Events to Amazon CloudWatch Logs](#insights-events-logging-CWL)

## Understanding Insights<a name="insights-events-understanding"></a>

CloudTrail Insights can help you detect unusual API activity in your AWS account by raising Insights events\. CloudTrail Insights tracks your normal patterns of APIs and generates Insights events when the API call volume is outside normal patterns\. Insights events are generated for `write` management APIs\.

After you enable CloudTrail Insights for the first time on a trail, it can take up to 36 hours for CloudTrail to deliver the first Insights event, if unusual activity is detected\. CloudTrail Insights analyzes write management events that occur in a single Region, not globally\. A CloudTrail Insights event is generated in the same Region as its supporting management events are generated\.

The following image shows an example of what Insights events look like\. You open details pages for an Insights event by choosing an Insights event name from the **Dashboard** or **Insights** pages\. The details page for an Insights event shows a graph of an API's call volume that occurred over a period of time before and after one or more Insights events are logged\. In the graph, Insights events are highlighted with vertical bars, with the width of the bar showing the start and end time of the Insights event\.

In this example, vertical highlighting bands show unusual numbers of AWS Systems Manager `UpdateInstanceInformation` API calls in an account\. In the highlighted area, because the number of `UpdateInstanceInformation` calls rose above the account's normal range of 100\.4 calls per minute, CloudTrail logged an Insights event when it detected the unusual activity\. The Insights event recorded that as many as 875 `UpdateInstanceInformation` calls were made at about 1:37 p\.m\. This is about 775 more calls to that API per minute than is expected for the account\. In this example, the graph's time span is one hour: 1:10 p\.m\. PDT on November 1, 2019 to 2:10 p\.m\. PDT on November 1, 2019\. This event has a start time of 1:37 p\.m\. PDT on November 1, 2019, and an end time of 1:38 p\.m\. PDT\.

The left column lists Insights events that are related to the subject API, and that have the same Insights event type\.

![\[A CloudTrail Insights detail page showing unusual API activity that was logged as an Insights event.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/insights_event_view.png)

On the **CloudTrail events** tab, view related events that CloudTrail analyzed to determine that unusual activity occurred\. Note that a filter is already applied for the Insights event name, which is also the name of the related API\. The **CloudTrail events** tab shows CloudTrail management events related to the subject API that occurred between the start and end time of the Insights event\. These events help you perform deeper analysis to determine the probable cause of an Insights event, and reasons for unusual API activity\.

If you disable CloudTrail Insights on a trail, or stop logging on a trail \(which disables CloudTrail Insights\), you may have Insights events stored in your destination S3 bucket, or shown on the **Insights** page of the console, that date from the earlier time that you had Insights enabled\.

## Logging Insights Events with the AWS Management Console<a name="insights-events-enable"></a>

Enable Insights event collection on an existing trail\. By default, Insights event collection is not enabled\.

1. Navigate to the **Trails** page of the CloudTrail console and choose a trail\.

1. In **Insights events**, choose the pencil icon\.

1. For **Log Insights events**, choose **Yes**\. Choose **Save** to save your changes\.

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

The following result shows the Insights event selector configured for the trail\.

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