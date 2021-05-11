# Getting and Viewing Your CloudTrail Log Files<a name="get-and-view-cloudtrail-log-files"></a>

 After you create a trail and configure it to capture the log files you want, you need to be able to find the log files and interpret the information they contain\.

CloudTrail delivers your log files to an Amazon S3 bucket that you specify when you create the trail\. CloudTrail typically delivers logs within an average of about 15 minutes of an API call\. This time is not guaranteed\. Review the [AWS CloudTrail Service Level Agreement](http://aws.amazon.com/cloudtrail/sla) for more information\. Insights events are typically delivered to your bucket within 30 minutes of unusual activity\. After you enable Insights events for the first time, allow up to 36 hours to see the first Insights events, if unusual activity is detected\.

**Topics**
+ [Finding Your CloudTrail Log Files](cloudtrail-find-log-files.md)
+ [Downloading Your CloudTrail Log Files](cloudtrail-read-log-files.md)