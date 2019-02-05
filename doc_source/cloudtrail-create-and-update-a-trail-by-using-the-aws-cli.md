# Creating and Updating a Trail with the AWS Command Line Interface<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli"></a>

**Note**  
You need the AWS command line tools to run the AWS Command Line Interface \(AWS CLI\) commands in this topic\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For help with CloudTrail commands at the AWS CLI command line, type `aws cloudtrail help`\.

**Contents**
+ [Two options for creating and updating trails](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-options)
  + [create\-trail and update\-trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-ctut)
  + [create\-subscription and update\-subscription](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-subs)
+ [Using create\-trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail)
  + [Creating a single\-region trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single)
    + [Start logging for the trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single-start-logging)
  + [Creating a trail that applies to all regions](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrt)
  + [Creating a trail that applies to all regions and that has log file validation enabled](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrtlfi)
+ [Using update\-trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail)
  + [Converting a trail that applies to one region to apply to all regions](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert)
  + [Converting a multi\-region trail to a single\-region trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce)
  + [Enabling and disabling logging global service events](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses)
  + [Enabling log file validation](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi)
  + [Disabling log file validation](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi-disable)
+ [Using create\-subscription](#cloudtrail-create-and-update-a-trail-cli-create-subscription)
+ [Using update\-subscription](#cloudtrail-create-and-update-a-trail-cli-update-subscription)
+ [Managing Trails](#cloudtrail-additional-cli-commands)
  + [Retrieving trail settings and the status of a trail](#cloudtrail-additional-cli-commands-retrieve)
  + [Configuring event selectors](#configuring-event-selector-examples)
    + [Example: A trail with specific event selectors](#configuring-event-selector-example1)
    + [Example: A trail that logs all events](#configuring-event-selector-example2)
  + [Stopping and starting logging for a trail](#cloudtrail-start-stop-logging-cli-commands)
  + [Deleting a trail](#cloudtrail-delete-trail-cli)

## Two options for creating and updating trails<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-options"></a>

When creating or updating a trail with the AWS CLI, you have two sets of options:
+ `create-trail` and `update-trail`
+ `create-subscription` and `update-subscription`

### create\-trail and update\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-ctut"></a>

The `create-trail` and `update-trail` commands offer the following functionality that the `create-subscription` and `update-subscription` commands do not:
+ Create a trail that receives logs across regions, or update a trail with the `--is-multi-region-trail` option\. 
+ Convert a multi\-region trail to single\-region trail with the `--no-is-multi-region-trail` option\.
+ Enable or disable log file encryption with the `--kms-key-id` option\. The option specifies an AWS KMS key that you have already created and to which you have attached a policy that allows CloudTrail to encrypt your logs\. For more information, see [Enabling and disabling CloudTrail log file encryption with the AWS CLI](cloudtrail-log-file-encryption-cli.md)\.
+ Enable or disable log file validation with the `--enable-log-file-validation` and `--no-enable-log-file-validation` options\. For more information, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.
+ Specify a CloudWatch Logs log group and role so that CloudTrail can deliver events to a CloudWatch Logs log group\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

### create\-subscription and update\-subscription<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-subs"></a>

The `create-subscription` and `update-subscription` commands offer the following advantages:
+ You can have CloudTrail create an S3 bucket for you\. With the `create-trail` command, you must specify an existing bucket in which you have already applied the bucket policy for CloudTrail\.
+ The `create-subscription` command starts logging for the trail\. With the `create-trail` command, you must run the `start-logging` command\.

## Using create\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail"></a>

### Creating a single\-region trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single"></a>

The following command creates a single\-region trail\. The specified Amazon S3 bucket must already exist and have the appropriate CloudTrail permissions applied\. For more information, see [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket
```

For more information, see [CloudTrail Trail Naming Requirements](cloudtrail-trail-naming-requirements.md)\.

Sample output:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false,
    "IsMultiRegionTrail": false,
    "IsOrganizationTrail": false, 
    "S3BucketName": "my-bucket"
}
```

By default, the `create-trail` command creates a single\-region trail and that trail does not enable log file validation\. 

#### Start logging for the trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single-start-logging"></a>

After the `create-trail` command completes, run the `start-logging` command to start logging for that trail\.

**Note**  
When you create a trail with the CloudTrail console or the `create-subscription` command, logging is turned on automatically\.

The following example starts logging for a trail:

```
aws cloudtrail start-logging --name my-trail
```

This command doesn't return an output, but you can use the `get-trail-status` command to verify that logging has started:

```
aws cloudtrail get-trail-status --name my-trail
```

To confirm that the trail is logging, the `IsLogging` element in the output shows `true`:

```
{
    "LatestDeliveryTime": 1441139757.497, 
    "LatestDeliveryAttemptTime": "2015-09-01T20:35:57Z", 
    "LatestNotificationAttemptSucceeded": "2015-09-01T20:35:57Z", 
    "LatestDeliveryAttemptSucceeded": "2015-09-01T20:35:57Z", 
    "IsLogging": true, 
    "TimeLoggingStarted": "2015-09-01T00:54:02Z", 
    "StartLoggingTime": 1441068842.76, 
    "LatestDigestDeliveryTime": 1441140723.629, 
    "LatestNotificationAttemptTime": "2015-09-01T20:35:57Z", 
    "TimeLoggingStopped": ""
}
```

### Creating a trail that applies to all regions<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrt"></a>

To create a trail that applies to all regions, use the `--is-multi-region-trail` option\. 

The following example creates a trail that delivers logs from all regions to an existing bucket named `my-bucket`:

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail
```

To confirm that your trail exists in all regions, the `IsMultiRegionTrail` element in the output shows `true`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

**Note**  
Use the `start-logging` command to start logging for your trail\.

### Creating a trail that applies to all regions and that has log file validation enabled<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrtlfi"></a>

To enable log file validation when using `create-trail`, use the `--enable-log-file-validation` option\.

For information about log file validation, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.

The following example creates a trail that delivers logs from all regions to the specified bucket\. The command uses the `--enable-log-file-validation` option\. 

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail --enable-log-file-validation
```

To confirm that log file validation is enabled, the `LogFileValidationEnabled` element in the output shows `true`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": true, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

## Using update\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail"></a>

You can use the `update-trail` command to change the configuration settings for a trail\. 

**Note**  
If you use the AWS CLI or one of the AWS SDKs to modify a trail, be sure that the trail's bucket policy is up\-to\-date\. In order for your bucket to automatically receive events from a new AWS Region, the policy must contain the full service name, `cloudtrail.amazonaws.com`\. For more information, see [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

**Note**  
You can run the `update-trail` command only from the region in which the trail was created\.

### Converting a trail that applies to one region to apply to all regions<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert"></a>

To change an existing trail so that it applies to all regions use the `--is-multi-region-trail` option:

```
aws cloudtrail update-trail --name my-trail --is-multi-region-trail
```

To confirm that the trail now applies to all regions, the `IsMultiRegionTrail` element in the output shows `true`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

### Converting a multi\-region trail to a single\-region trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce"></a>

To change an existing multi\-region trail so that it appliesonly to the region in which it was created, use the `--no-is-multi-region-trail` option\. 

```
aws cloudtrail update-trail --name my-trail --no-is-multi-region-trail
```

To confirm that the trail now applies to a single region, the `IsMultiRegionTrail` element in the output shows `false`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": false, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

### Enabling and disabling logging global service events<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses"></a>

To change a trail so that it does not log global service events, use the `--no-include-global-service-events` option\. 

```
aws cloudtrail update-trail --name my-trail --no-include-global-service-events
```

To confirm that the trail no longer logs global service events, the `IncludeGlobalServiceEvents` element in the output shows `false`:

```
{
    "IncludeGlobalServiceEvents": false, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": false, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

To change a trail so that it logs global service events, use the `--include-global-service-events` option\.

### Enabling log file validation<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi"></a>

To enable log file validation for a trail, use the `--enable-log-file-validation` option\. Digest files are delivered to the Amazon S3 bucket for that trail\.

```
aws cloudtrail update-trail --name my-trail --enable-log-file-validation
```

To confirm that log file validation is enabled, the `LogFileValidationEnabled` element in the output shows `true`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": true, 
    "IsMultiRegionTrail": false, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

### Disabling log file validation<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi-disable"></a>

To disable log file validation for a trail, use the `--no-enable-log-file-validation` option\.

```
aws cloudtrail update-trail --name my-trail-name --no-enable-log-file-validation
```

To confirm that log file validation is disabled, the `LogFileValidationEnabled` element in the output shows `false`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": false, 
    "IsOrganizationTrail": false,
    "S3BucketName": "my-bucket"
}
```

To validate log files with the AWS CLI, see [Validating CloudTrail Log File Integrity with the AWS CLI](cloudtrail-log-file-validation-cli.md)\.

## Using create\-subscription<a name="cloudtrail-create-and-update-a-trail-cli-create-subscription"></a>

The `create-subscription` command creates a trail\. You can also use this command to create an Amazon S3 bucket for log file delivery and an Amazon SNS topic for notifications\. The `create-subscription` command also starts logging for the trail that it creates\. 

The `create-subscription` command includes the following options: 
+ `--name` specifies the name of the trail\. This option is required\. For more information, see [CloudTrail Trail Naming Requirements](cloudtrail-trail-naming-requirements.md)\.
+  `--s3-use-bucket` specifies an existing Amazon S3 bucket for log file storage\.
+  `--s3-new-bucket` specifies the name of the new bucket created when the command executes\. The name of the bucket must be globally unique\. For more information, see [Amazon S3 Bucket Naming Requirements](cloudtrail-s3-bucket-naming-requirements.md)\.
+  `--s3-prefix` specifies a prefix for the log file delivery path \(optional\)\. The maximum length is 200 characters\.
**Note**  
If you want to use a new log file prefix for an existing bucket, add the prefix to the bucket policy first\. For more information, see [Changing a Prefix for an Existing Bucket](create-s3-bucket-policy-for-cloudtrail.md#cloudtrail-add-change-or-remove-a-bucket-prefix)\.
+  `--sns-new-topic` specifies the name of the Amazon SNS topic to which you can subscribe for notification of log file delivery to your bucket \(optional\)\.

**Note**  
Type `aws cloudtrail create-subscription help` to see the list of options\.

The following example creates a trail, an Amazon S3 bucket for log file delivery, an S3 bucket prefix, and an SNS topic\.

```
aws cloudtrail create-subscription --name=awscloudtrail-example --s3-new-bucket=awscloudtrail-new-bucket-example --s3-prefix=prefix-example --sns-new-topic=awscloudtrail-example-log-deliverytopic
```

 If the command executes successfully, you see output similar to the following:

```
Setting up new S3 bucket awscloudtrail-new-bucket-example...
Setting up new SNS topic awscloudtrail-example-log-deliverytopic...
Creating/updating CloudTrail configuration...
CloudTrail configuration: 
{
  "trailList": [
    {
      "IncludeGlobalServiceEvents": true,
      "Name": "awscloudtrail-example",
      "S3KeyPrefix": "prefix-example",
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/awscloudtrail-example",
      "LogFileValidationEnabled": false, 
      "IsMultiRegionTrail": false,
      "IsOrganizationTrail": false,
      "HasCustomEventSelectors": false,  
      "S3BucketName": "awscloudtrail-new-bucket-example"
      "SnsTopicName": "awscloudtrail-example-log-deliverytopic",
      "HomeRegion": "us-east-2"
    }
  ],
  "ResponseMetadata": {
      "HTTPStatusCode": 200,
      "RequestId": "4c55c744-a0ea-4aea-b3b9-eb63dfe68383"
   }
}
Starting CloudTrail service...
Logs will be delivered to awscloudtrail-new-bucket-example:prefix-example
```

## Using update\-subscription<a name="cloudtrail-create-and-update-a-trail-cli-update-subscription"></a>

You can update your trail by using the `update-subscription` command and setting the options to new values\. The following example uses the `--s3-use-bucket` option to designate a different, existing Amazon S3 bucket\. If you want a trail with a different name, delete the trail with the `delete-trail` command and then run the `create-subscription` command\.

```
aws cloudtrail update-subscription --name=awscloudtrail-example --s3-use-bucket=awscloudtrail-new-bucket-example2 --s3-prefix=prefix-example
```

If the command executes successfully, the `S3BucketName` value is updated to *awscloudtrail\-new\-bucket\-example2*:

```
CloudTrail configuration:
{
  "trailList": [
    {
      "IncludeGlobalServiceEvents": true,
      "Name": "awscloudtrail-example",
      "S3KeyPrefix": "prefix-example",
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/awscloudtrail-example",
      "LogFileValidationEnabled": false,
      "IsMultiRegionTrail": false,
      "IsOrganizationTrail": false,
      "HasCustomEventSelectors": false,  
      "S3BucketName": "awscloudtrail-new-bucket-example2"
      "SnsTopicName": "awscloudtrail-example-log-deliverytopic",
      "HomeRegion": "us-east-2"
    }
  ]
}
```

**Note**  
If you specify an existing Amazon S3 bucket and that bucket was not created with CloudTrail, you need to attach the appropriate policy\. See [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

## Managing Trails<a name="cloudtrail-additional-cli-commands"></a>

 The CloudTrail CLI includes several other commands that help you manage your trails\.

### Retrieving trail settings and the status of a trail<a name="cloudtrail-additional-cli-commands-retrieve"></a>

Use the `describe-trails` command to retrieve trail settings: 

```
aws cloudtrail describe-trails
```

If the command succeeds, you see output similar to the following: 

```
{
  "trailList": [
    {
      "IncludeGlobalServiceEvents": true,
      "Name": "my-trail",
      "S3KeyPrefix": "my-prefix",
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail",
      "LogFileValidationEnabled": false,
      "IsMultiRegionTrail": false,
      "IsOrganizationTrail": false,
      "HasCustomEventSelectors": false, 
      "S3BucketName": "my-bucket"
      "SnsTopicName": "my-topic",
      "HomeRegion": "us-east-2"
    }
  ]
}
```

Run the `get-trail-status` command to retrieve the status of a trail\. 

**Note**  
If the trail is an organization trail and you are a member account in the organization in AWS Organizations, you must provide the full ARN of that trail, and not just the name\.

```
aws cloudtrail get-trail-status --name awscloudtrail-example
```

If the command succeeds, you see output similar to the following: 

```
{
    "LatestDeliveryTime": 1441139757.497, 
    "LatestDeliveryAttemptTime": "2015-09-01T20:35:57Z", 
    "LatestNotificationAttemptSucceeded": "2015-09-01T20:35:57Z", 
    "LatestDeliveryAttemptSucceeded": "2015-09-01T20:35:57Z", 
    "IsLogging": true, 
    "TimeLoggingStarted": "2015-09-01T00:54:02Z", 
    "StartLoggingTime": 1441068842.76, 
    "LatestDigestDeliveryTime": 1441140723.629, 
    "LatestNotificationAttemptTime": "2015-09-01T20:35:57Z", 
    "TimeLoggingStopped": ""
}
```

In addition to the fields shown in the preceding JSON code, the status contains the following fields if there are Amazon SNS or Amazon S3 errors: 
+ `LatestNotificationError`\. Contains the error emitted by Amazon SNS if a subscription to a topic fails\.
+ `LatestDeliveryError`\. Contains the error emitted by Amazon S3 if CloudTrail cannot deliver a log file to a bucket\.

### Configuring event selectors<a name="configuring-event-selector-examples"></a>

To view the event selector settings for a trail, run the `get-event-selectors` command: 

```
aws cloudtrail get-event-selectors --trail-name TrailName
```

**Note**  
If the trail is an organization trail and you are a member account in the organization in AWS Organizations, you must provide the full ARN of that trail, and not just the name\.

The following example returns the default settings for an event selector for a trail\.

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

To create an event selector, run the `put-event-selectors` command\. When an event occurs in your account, CloudTrail evaluates the configuration for your trails\. If the event matches any event selector for a trail, the trail processes and logs the event\. You can configure up to 5 event selectors for a trail and up to 250 data resources for a trail\. For more information, see [Logging Data and Management Events for Trails](logging-management-and-data-events-with-cloudtrail.md)\.

**Topics**
+ [Example: A trail with specific event selectors](#configuring-event-selector-example1)
+ [Example: A trail that logs all events](#configuring-event-selector-example2)

#### Example: A trail with specific event selectors<a name="configuring-event-selector-example1"></a>

The following example creates an event selector for a trail named *TrailName* to include read\-only and write\-only management events, data events for two Amazon S3 bucket/prefix combinations, and data events for a single AWS Lambda function named *hello\-world\-python\-function*\. 

```
aws cloudtrail put-event-selectors --trail-name TrailName --event-selectors '[{"ReadWriteType": "All","IncludeManagementEvents": true,"DataResources": [{"Type":"AWS::S3::Object", "Values": ["arn:aws:s3:::mybucket/prefix","arn:aws:s3:::mybucket2/prefix2"]},{"Type": "AWS::Lambda::Function","Values": ["arn:aws:lambda:us-west-2:999999999999:function:hello-world-python-function"]}]}]'
```

The example returns the event selector configured for the trail:

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [
                {
                    "Values": [
                        "arn:aws:s3:::mybucket/prefix",
                        "arn:aws:s3:::mybucket2/prefix2"
                    ],
                    "Type": "AWS::S3::Object"
                },
                {
                    "Values": [
                        "arn:aws:lambda:us-west-2:123456789012:function:hello-world-python-function"
                    ],
                    "Type": "AWS::Lambda::Function"
                },
            ],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

#### Example: A trail that logs all events<a name="configuring-event-selector-example2"></a>

The following example creates an event selector for a trail named *TrailName2* that includes all events, including read\-only and write\-only management events, and all data events for all Amazon S3 buckets and AWS Lambda functions in the AWS account\.

**Note**  
If the trail applies only to one region, only events in that region are logged, even though the event selector parameters specify all Amazon S3 buckets and Lambda functions\. Event selectors apply only to the regions where the trail is created\.

```
aws cloudtrail put-event-selectors --trail-name TrailName2 --event-selectors '[{"ReadWriteType": "All","IncludeManagementEvents": true,"DataResources": [{"Type":"AWS::S3::Object", "Values": ["arn:aws:s3:::"]},{"Type": "AWS::Lambda::Function","Values": ["arn:aws:lambda"]}]}]'
```

The example returns the event selectors configured for the trail:

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [
                {
                    "Values": [
                        "arn:aws:s3:::"
                    ],
                    "Type": "AWS::S3::Object"
                },
                {
                    "Values": [
                        "arn:aws:lambda"
                    ],
                    "Type": "AWS::Lambda::Function"
                },
            ],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName2"
}
```

### Stopping and starting logging for a trail<a name="cloudtrail-start-stop-logging-cli-commands"></a>

The following commands start and stop CloudTrail logging:

```
aws cloudtrail start-logging --name awscloudtrail-example
```

```
aws cloudtrail stop-logging --name awscloudtrail-example
```

**Note**  
Before deleting a bucket, run the `stop-logging` command to stop delivering events to the bucket\. If you don’t stop logging, CloudTrail attempts to deliver log files to a bucket with the same name for a limited period of time\.

### Deleting a trail<a name="cloudtrail-delete-trail-cli"></a>

You can delete a trail with the following command\. You can delete a trail only in the region it was created\. 

```
aws cloudtrail delete-trail --name awscloudtrail-example
```

When you delete a trail, you do not delete the Amazon S3 bucket or the Amazon SNS topic associated with it\. Use the AWS Management Console, AWS CLI, or service API to delete these resources separately\.