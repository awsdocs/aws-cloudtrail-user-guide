# CloudWatch log group and log stream naming for CloudTrail<a name="cloudwatch-log-group-log-stream-naming-for-cloudtrail"></a>

Amazon CloudWatch will display the log group that you created for CloudTrail events alongside any other log groups you have in a region\. We recommend that you use a log group name that helps you easily distinguish the log group from others\. For example, **CloudTrail/logs**\.

Follow these guidelines when naming a log group:
+ Log group names must be unique within a region for an AWS account\.
+ Log group names can be between 1 and 512 characters long\.
+ Log group names consist of the following characters: a\-z, A\-Z, 0\-9, '\_' \(underscore\), '\-' \(hyphen\), '/' \(forward slash\), '\.' \(period\), and '\#' \(number sign\)\.

When CloudTrail creates the log stream for the log group, it names the log stream according to the following format: *account\_ID*\_CloudTrail\_*source\_region*\.

**Note**  
If the volume of CloudTrail logs is large, multiple log streams may be created to deliver log data to your log group\.

For more information about CloudWatch log groups, see [Working with log groups and log streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in the *Amazon CloudWatch Logs User Guide* and [CreateLogGroup](https://docs.aws.amazon.com/AmazonCloudWatchLogs/latest/APIReference/API_CreateLogGroup.html) in the *Amazon CloudWatch Logs API Reference*\.