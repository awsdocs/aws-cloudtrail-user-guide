# Creating a Trail For Your AWS Account<a name="cloudtrail-create-and-update-a-trail"></a>

When you create a trail, you enable ongoing delivery of events as log files to an Amazon S3 bucket that you specify\. Creating a trail has many benefits, including:
+ A record of events that extends past 90 days\.
+ The option to automatically monitor and alarm on specified events by sending log events to Amazon CloudWatch Logs\. 
+ The option to query logs and analyze AWS service activity with Amazon Athena\.

Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.

If you use AWS Organizations, you can create a trail that will log events for all AWS accounts in the organization\. A trail with the same name will be created in each member account, and events from each trail will be delivered to the Amazon S3 bucket that you specify\. 

**Note**  
Only the master account for an organization can create a trail for the organization\. Creating a trail for an organization automatically enables integration between CloudTrail and Organizations\. For more information, see [Creating a Trail for an Organization](creating-trail-organization.md)\.

You can configure the following settings when you create or update a trail with the CloudTrail console or the AWS Command Line Interface \(AWS CLI\)\. Both methods follow the same steps: 

1. Create a trail\. By default, when you create a trail in a region in the CloudTrail console, the trail applies to all regions\.

1. Create an Amazon S3 bucket or specify an existing bucket where you want the log files delivered\. By default, log files from all regions in your account are delivered to the bucket that you specify\.

1. Configure your trail to log read\-only, write\-only, or all management events, and all or a subset of data events\. By default, trails log all management events and no data events\.

1. Create an Amazon SNS topic to receive notifications when log files are delivered\. Delivery notifications from all regions are sent to the topic that you specify\.

1. Configure CloudWatch Logs to receive your logs from CloudTrail so that you can monitor for specific log events\. 

1. Change the encryption method for your log files from server\-side encryption with Amazon S3\-managed encryption keys \(SSE\-S3\) to server\-side encryption with AWS KMS–managed keys \(SSE\-KMS\)\. 

1. Turn on integrity validation for log files\. This enables the delivery of digest files that you can use to validate the integrity of log files after CloudTrail has delivered them\.

1. Add tags \(custom key\-value pairs\) to your trail\.

**Topics**
+ [Creating and Updating a Trail with the Console](cloudtrail-create-and-update-a-trail-by-using-the-console.md)
+ [Creating and Updating a Trail with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)