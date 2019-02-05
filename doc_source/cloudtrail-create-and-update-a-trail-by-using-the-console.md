# Creating and Updating a Trail with the Console<a name="cloudtrail-create-and-update-a-trail-by-using-the-console"></a>

You can create, update, or delete your trails with the CloudTrail console\. You can create up to five trails for each region\. After you create a trail, CloudTrail automatically starts logging API calls and related events in your account to the Amazon S3 bucket that you specify\. To stop logging, you can turn off logging for the trail or delete it\.

Creating and updating trails in the CloudTrail console has several features not available when creating a trail using the AWS CLI\. For example, if you are configuring a trail to log data events for Amazon S3 buckets or AWS Lambda functions in your AWS account, you can view a list of these resources while creating the trail in the console experience\. If this is your first time creating a trail, creating a trail using the console will help you understand the features and options available for the trail\.

For information specific to creating a trail for an organization in AWS Organizations, see [Creating a Trail for an Organization](creating-trail-organization.md)\.

**Topics**
+ [Creating a Trail](cloudtrail-create-a-trail-using-the-console-first-time.md)
+ [Updating a Trail](cloudtrail-update-a-trail-console.md)
+ [Deleting a Trail](cloudtrail-delete-trails-console.md)
+ [Turning off Logging for a Trail](cloudtrail-turning-off-logging.md)