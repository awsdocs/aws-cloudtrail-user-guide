# Receiving CloudTrail log files from multiple accounts<a name="cloudtrail-receive-logs-from-multiple-accounts"></a>

You can have CloudTrail deliver log files from multiple AWS accounts into a single Amazon S3 bucket\. For example, you have four AWS accounts with account IDs 111111111111, 222222222222, 333333333333, and 444444444444, and you want to configure CloudTrail to deliver log files from all four of these accounts to a bucket belonging to account 111111111111\. To accomplish this, complete the following steps in order:

1. Turn on CloudTrail in the account where the destination bucket will belong \(111111111111 in this example\)\. Do not turn on CloudTrail in any other accounts yet\.

   For instructions, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md)\.

1. Update the bucket policy on your destination bucket to grant cross\-account permissions to CloudTrail\.

   For instructions, see [Setting bucket policy for multiple accounts](cloudtrail-set-bucket-policy-for-multiple-accounts.md)\.

1. Turn on CloudTrail in the other accounts you want \(222222222222, 333333333333, and 444444444444 in this example\)\. Configure CloudTrail in these accounts to use the same bucket belonging to the account that you specified in step 1 \(111111111111 in this example\)\.

   For instructions, see [Turning on CloudTrail in additional accounts](turn-on-cloudtrail-in-additional-accounts.md)\.

## Redacting bucket owner account IDs for data events called by other accounts<a name="cloudtrail-receive-logs-s3-owner-id-redaction"></a>

Historically, if CloudTrail data events were enabled in the AWS account of an Amazon S3 data event API caller, CloudTrail showed the account ID of the S3 bucket owner in the data event \(such as `PutObject`\)\. This occurred even if the bucket owner account did not have S3 data events enabled\.

Now, CloudTrail removes the account ID of the S3 bucket owner in the `resources` block if both of the following conditions are met:
+ The data event API call is from a different AWS account than the Amazon S3 bucket owner\.
+ The API caller received an `AccessDenied` error that was only for the caller account\.

The owner of the resource on which the API call was made still receives the full event\.

The following event record snippets are an example of the expected behavior\. In the `Historic` snippet, the account ID 123456789012 of the S3 bucket owner is shown to an API caller from a different account\. In the example of current behavior, the account ID of the bucket owner is not shown\.

```
# Historic

"resources": [
    {
        "type": "AWS::S3::Object",
        "ARNPrefix": "arn:aws:s3:::test-my-bucket-2/"
    },
    {
        "accountId": "123456789012",
        "type": "AWS::S3::Bucket",
        "ARN": "arn:aws:s3:::test-my-bucket-2"
    }
]
```

The following is the current behavior\.

```
# Current

"resources": [
    {
        "type": "AWS::S3::Object",
        "ARNPrefix": "arn:aws:s3:::test-my-bucket-2/"
    },
    {
        "accountId": "",
        "type": "AWS::S3::Bucket",
        "ARN": "arn:aws:s3:::test-my-bucket-2"
    }
]
```

**Topics**
+ [Redacting bucket owner account IDs for data events called by other accounts](#cloudtrail-receive-logs-s3-owner-id-redaction)
+ [Setting bucket policy for multiple accounts](cloudtrail-set-bucket-policy-for-multiple-accounts.md)
+ [Turning on CloudTrail in additional accounts](turn-on-cloudtrail-in-additional-accounts.md)