# Working with CloudTrail<a name="cloudtrail-getting-started"></a>

 CloudTrail is enabled by default for your AWS account\. You can use **Event history** in the CloudTrail console to view, search, download, archive, analyze, and respond to account activity across your AWS infrastructure\. This includes activity made through the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs\.

For an ongoing record of events in your AWS account, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. 

If you have created an organization in AWS Organizations, you can create a trail that will log all events for all AWS accounts in that organization\. Creating an organization trail helps you define a uniform event logging strategy for your organization\.

**Topics**
+ [Viewing Events with CloudTrail Event History](view-cloudtrail-events.md)
+ [Creating a Trail For Your AWS Account](cloudtrail-create-and-update-a-trail.md)
+ [Creating a Trail for an Organization](creating-trail-organization.md)
+ [Getting and Viewing Your CloudTrail Log Files](get-and-view-cloudtrail-log-files.md)
+ [Configuring Amazon SNS Notifications for CloudTrail](configure-sns-notifications-for-cloudtrail.md)
+ [Controlling User Permissions for CloudTrail](control-user-permissions-for-cloudtrail.md)
+ [Tips for Managing Trails](cloudtrail-concepts-trails-managing-and-using.md)
+ [Using AWS CloudTrail with Interface VPC Endpoints](cloudtrail-and-interface-VPC.md)