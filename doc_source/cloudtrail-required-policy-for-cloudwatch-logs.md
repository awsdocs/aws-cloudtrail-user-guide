# Role policy document for CloudTrail to use CloudWatch Logs for monitoring<a name="cloudtrail-required-policy-for-cloudwatch-logs"></a>

This section describes the permissions policy required for the CloudTrail role to send log events to CloudWatch Logs\. You can attach a policy document to a role when you configure CloudTrail to send events, as described in [Sending events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\. You can also create a role using IAM\. For more information, see [Creating a Role for an AWS Service \(AWS Management Console\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/create-role-xacct.html) or [Creating a Role \(CLI and API\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_CreateRole_CLIAPI.html)\.

The following example policy document contains the permissions required to create a CloudWatch log stream in the log group that you specify and to deliver CloudTrail events to that log stream in the US East \(Ohio\) region\. \(This is the default policy for the default IAM role `CloudTrail_CloudWatchLogs_Role`\.\)

```
{
  "Version": "2012-10-17",
  "Statement": [
    {

      "Sid": "AWSCloudTrailCreateLogStream2014110",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogStream"
      ],
      "Resource": [
        "arn:aws:logs:us-east-2:accountID:log-group:log_group_name:log-stream:CloudTrail_log_stream_name_prefix*"
      ]

    },
    {
      "Sid": "AWSCloudTrailPutLogEvents20141101",
      "Effect": "Allow",
      "Action": [
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:us-east-2:accountID:log-group:log_group_name:log-stream:CloudTrail_log_stream_name_prefix*"
      ]
    }
  ]
}
```

If you're creating a policy that might be used for organization trails as well, you will need to modify it from the default policy created for the role\. For example, the following policy grants CloudTrail the permissions required to create a CloudWatch Logs log stream in the log group you specify as the value of *log\_group\_name*, and to deliver CloudTrail events to that log stream for both trails in the AWS account 111111111111 and for organization trails created in the 111111111111 account that are applied to the AWS Organizations organization with the ID of *o\-exampleorgid*:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailCreateLogStream20141101",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream"
            ],
            "Resource": [
                "arn:aws:logs:us-east-2:111111111111:log-group:log_group_name:log-stream:111111111111_CloudTrail_us-east-2*",
                "arn:aws:logs:us-east-2:111111111111:log-group:log_group_name:log-stream:o-exampleorgid_*"
            ]
        },
        {
            "Sid": "AWSCloudTrailPutLogEvents20141101",
            "Effect": "Allow",
            "Action": [
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:us-east-2:111111111111:log-group:log_group_name:log-stream:111111111111_CloudTrail_us-east-2*",
                "arn:aws:logs:us-east-2:111111111111:log-group:log_group_name:log-stream:o-exampleorgid_*"
            ]
        }
    ]
}
```

For more information about organization trails, see [Creating a trail for an organization](creating-trail-organization.md)\.