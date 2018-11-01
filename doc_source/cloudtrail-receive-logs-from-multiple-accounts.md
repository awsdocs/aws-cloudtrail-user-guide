# Receiving CloudTrail Log Files from Multiple Accounts<a name="cloudtrail-receive-logs-from-multiple-accounts"></a>

You can have CloudTrail deliver log files from multiple AWS accounts into a single Amazon S3 bucket\. For example, you have four AWS accounts with account IDs 111111111111, 222222222222, 333333333333, and 444444444444, and you want to configure CloudTrail to deliver log files from all four of these accounts to a bucket belonging to account 111111111111\. To accomplish this, complete the following steps in order:

1. Turn on CloudTrail in the account where the destination bucket will belong \(111111111111 in this example\)\. Do not turn on CloudTrail in any other accounts yet\.

   For instructions, see [Creating a Trail](cloudtrail-create-a-trail-using-the-console-first-time.md)\. 

1. Update the bucket policy on your destination bucket to grant cross\-account permissions to CloudTrail\. 

   For instructions, see [Setting Bucket Policy for Multiple Accounts](cloudtrail-set-bucket-policy-for-multiple-accounts.md)\. 

1. Turn on CloudTrail in the other accounts you want \(222222222222, 333333333333, and 444444444444 in this example\)\. Configure CloudTrail in these accounts to use the same bucket belonging to the account that you specified in step 1 \(111111111111 in this example\)\. 

   For instructions, see [Turning on CloudTrail in Additional Accounts](turn-on-cloudtrail-in-additional-accounts.md)\. 

**Topics**
+ [Setting Bucket Policy for Multiple Accounts](cloudtrail-set-bucket-policy-for-multiple-accounts.md)
+ [Turning on CloudTrail in Additional Accounts](turn-on-cloudtrail-in-additional-accounts.md)