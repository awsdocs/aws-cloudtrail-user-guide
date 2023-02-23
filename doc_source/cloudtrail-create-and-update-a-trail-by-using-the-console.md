# Creating and updating a trail with the console<a name="cloudtrail-create-and-update-a-trail-by-using-the-console"></a>

You can use the CloudTrail console to create, update, or delete your trails\. Trails created using the console are multi\-Region\. To create a trail that logs events in only one AWS Region, [use the AWS CLI](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single)\.

You can create up to five trails for each Region\. After you create a trail, CloudTrail automatically starts logging API calls and related events in your account to the Amazon S3 bucket that you specify\. To stop logging, you can turn off logging for the trail or delete it\.

Using the CloudTrail console to create or update a trail provides the following advantages\.
+ If this is your first time creating a trail, using the CloudTrail console let's you view the available feature and options\.
+ If you are configuring a trail to log data events, using the CloudTrail console let's you view the available data types\. For more information about logging data events, see [Logging data events](logging-data-events-with-cloudtrail.md)\.

For information specific to creating a trail for an organization in AWS Organizations, see [Creating a trail for an organization](creating-trail-organization.md)\.

**Topics**
+ [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md)
+ [Updating a trail](cloudtrail-update-a-trail-console.md)
+ [Deleting a trail](cloudtrail-delete-trails-console.md)
+ [Turning off logging for a trail](cloudtrail-turning-off-logging.md)