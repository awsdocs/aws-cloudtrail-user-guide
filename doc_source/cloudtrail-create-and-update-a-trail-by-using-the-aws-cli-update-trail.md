# Using update\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail"></a>

You can use the `update-trail` command to change the configuration settings for a trail\. You can also use the add\-tags and remove\-tags commands to add and remove tags for a trail\. You can only update trails from the AWS Region where the trail was created \(its Home Region\)\. When using the AWS CLI, remember that your commands run in the AWS Region configured for your profile\. If you want to run the commands in a different Region, either change the default Region for your profile, or use the \-\-region parameter with the command\.

**Note**  
If you use the AWS CLI or one of the AWS SDKs to modify a trail, be sure that the trail's bucket policy is up\-to\-date\. In order for your bucket to automatically receive events from a new AWS Region, the policy must contain the full service name, `cloudtrail.amazonaws.com`\. For more information, see [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

## Converting a trail that applies to one Region to apply to all Regions<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert"></a>

To change an existing trail so that it applies to all Regions, use the `--is-multi-region-trail` option\.

```
aws cloudtrail update-trail --name my-trail --is-multi-region-trail
```

To confirm that the trail now applies to all Regions, the `IsMultiRegionTrail` element in the output shows `true`\.

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

## Converting a multi\-region trail to a single\-region trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce"></a>

To change an existing multi\-region trail so that it applies only to the Region in which it was created, use the `--no-is-multi-region-trail` option\. 

```
aws cloudtrail update-trail --name my-trail --no-is-multi-region-trail
```

To confirm that the trail now applies to a single Region, the `IsMultiRegionTrail` element in the output shows `false`\.

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

## Enabling and disabling logging global service events<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses"></a>

To change a trail so that it does not log global service events, use the `--no-include-global-service-events` option\. 

```
aws cloudtrail update-trail --name my-trail --no-include-global-service-events
```

To confirm that the trail no longer logs global service events, the `IncludeGlobalServiceEvents` element in the output shows `false`\.

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

## Enabling log file validation<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi"></a>

To enable log file validation for a trail, use the `--enable-log-file-validation` option\. Digest files are delivered to the Amazon S3 bucket for that trail\.

```
aws cloudtrail update-trail --name my-trail --enable-log-file-validation
```

To confirm that log file validation is enabled, the `LogFileValidationEnabled` element in the output shows `true`\.

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

## Disabling log file validation<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi-disable"></a>

To disable log file validation for a trail, use the `--no-enable-log-file-validation` option\.

```
aws cloudtrail update-trail --name my-trail-name --no-enable-log-file-validation
```

To confirm that log file validation is disabled, the `LogFileValidationEnabled` element in the output shows `false`\.

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