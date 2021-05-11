# Viewing Events with CloudTrail Event History<a name="view-cloudtrail-events"></a>

You can troubleshoot operational and security incidents over the past 90 days in the CloudTrail console by viewing **Event history**\. You can look up events related to creation, modification, or deletion of resources \(such as IAM users or Amazon EC2 instances\) in your AWS account on a per\-region basis\. Events can be viewed and downloaded by using the AWS CloudTrail console\. You can customize the view of event history in the console by selecting which columns are displayed and which are hidden\. You can programmatically look up events by using the AWS SDKs or AWS Command Line Interface\. You can also compare the details of events in **Event history** side\-by\-side\.

**Note**  
Over time, AWS services might add additional events\. CloudTrail will record these events in **Event history**, but a full 90\-day record of activity that includes added events will not be available until 90 days after the events are added\.

This section describes how to look up events by using the CloudTrail console and the AWS CLI\. It also describes how to download a file of events\. For information on using the `LookupEvents` API to retrieve information from CloudTrail events, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

For information on creating a trail so that you have a record of events that extends past 90 days, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Getting and Viewing Your CloudTrail Log Files](get-and-view-cloudtrail-log-files.md)\.

**Topics**
+ [Viewing CloudTrail events in the CloudTrail console](view-cloudtrail-events-console.md)
+ [Viewing CloudTrail Events with the AWS CLI](view-cloudtrail-events-cli.md)