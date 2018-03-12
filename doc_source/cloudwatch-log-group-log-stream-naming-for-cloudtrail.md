# CloudWatch Log Group and Log Stream Naming for CloudTrail<a name="cloudwatch-log-group-log-stream-naming-for-cloudtrail"></a>

Amazon CloudWatch will display the log group that you created for CloudTrail events alongside any other log groups you have in a region\. We recommend that you use a log group name that helps you easily distinguish the log group from others\. For example, **CloudTrail/logs**\. Log group names can be between 1 and 512 characters long\. Allowed characters include a\-z, A\-Z, 0\-9, '\_' \(underscore\), '\-' \(hyphen\), '/' \(forward slash\), and '\.' \(period\)\.

When CloudTrail creates the log stream for the log group, it names the log stream according to the following format: *account\_ID*\_CloudTrail\_*source\_region*\.

**Note**  
If the volume of CloudTrail logs is large, multiple log streams may be created to deliver log data to your log group\.