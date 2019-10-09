# Using create\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail"></a>

You can use the `create-trail` command to create trails that are specifically configured to meet your business needs\. When using the AWS CLI, remember that your commands run in the AWS Region configured for your profile\. If you want to run the commands in a different Region, either change the default Region for your profile, or use the \-\-region parameter with the command\.

## Creating a trail that applies to all Regions<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrt"></a>

To create a trail that applies to all Regions, use the `--is-multi-region-trail` option\. By default, the `create-trail` command creates a trail that logs events only in the AWS Region where the trail was created\. To ensure that you log global service events and capture all management event activity in your AWS account, you should create trails that log events in all AWS Regions\.

**Note**  
When creating a trail, if you specify an Amazon S3 bucket that was not created with CloudTrail, you need to attach the appropriate policy\. See [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

The following example creates a trail with the name *my\-trail* and a tag with a key named *Group* with a value of *Marketing* that delivers logs from all Regions to an existing bucket named `my-bucket`\.

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail --tags-list [key=Group,value=Marketing]
```

To confirm that your trail exists in all Regions, the `IsMultiRegionTrail` element in the output shows `true`\.

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

## Start logging for the trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single-start-logging"></a>

After the `create-trail` command completes, run the `start-logging` command to start logging for that trail\.

**Note**  
When you create a trail with the CloudTrail console, logging is turned on automatically\.

The following example starts logging for a trail\.

```
aws cloudtrail start-logging --name my-trail
```

This command doesn't return an output, but you can use the `get-trail-status` command to verify that logging has started\.

```
aws cloudtrail get-trail-status --name my-trail
```

To confirm that the trail is logging, the `IsLogging` element in the output shows `true`\.

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

## Creating a single\-region trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single"></a>

The following command creates a single\-region trail\. The specified Amazon S3 bucket must already exist and have the appropriate CloudTrail permissions applied\. For more information, see [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket
```

For more information, see [CloudTrail Trail Naming Requirements](cloudtrail-trail-naming-requirements.md)\.

The following is example output\.

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

## Creating a trail that applies to all Regions and that has log file validation enabled<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrtlfi"></a>

To enable log file validation when using `create-trail`, use the `--enable-log-file-validation` option\.

For information about log file validation, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.

The following example creates a trail that delivers logs from all Regions to the specified bucket\. The command uses the `--enable-log-file-validation` option\. 

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail --enable-log-file-validation
```

To confirm that log file validation is enabled, the `LogFileValidationEnabled` element in the output shows `true`\.

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