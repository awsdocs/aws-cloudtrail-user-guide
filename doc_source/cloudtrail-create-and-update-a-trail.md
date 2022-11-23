# Creating a trail for your AWS account<a name="cloudtrail-create-and-update-a-trail"></a>

When you create a trail, you enable ongoing delivery of events as log files to an Amazon S3 bucket that you specify\. Creating a trail has many benefits, including:
+ A record of events that extends past 90 days\.
+ The option to automatically monitor and alarm on specified events by sending log events to Amazon CloudWatch Logs\.
+ The option to query logs and analyze AWS service activity with Amazon Athena\.

Beginning on April 12, 2019, you can view trails only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it appears in the console in all Regions\. If you create a trail that only logs events in a single Region, you can view and manage it only in that Region\. Creating a multi\-region trail is the default option if you create a trail by using the AWS CloudTrail console, and is a recommended best practice\. To create a single\-region trail, you must use the AWS CLI\.

If you use AWS Organizations, you can create a trail that will log events for all AWS accounts in the organization\. A trail with the same name will be created in each member account, and events from each trail will be delivered to the Amazon S3 bucket that you specify\. 

**Note**  
Only the management account or delegated administrator account for an organization can create a trail for the organization\. Creating a trail for an organization automatically enables integration between CloudTrail and Organizations\. For more information, see [Creating a trail for an organization](creating-trail-organization.md)\.

**Topics**
+ [Creating and updating a trail with the console](cloudtrail-create-and-update-a-trail-by-using-the-console.md)
+ [Creating, updating, and managing trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)