# Using update\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail"></a>

**Important**  
As of November 22, 2021, AWS CloudTrail changed how trails capture global service events\. Now, events created by Amazon CloudFront, AWS Identity and Access Management, and AWS STS are recorded in the region in which they were created, the US East \(N\. Virginia\) region, us\-east\-1\. This makes CloudTrail's treatment of these services consistent with that of other AWS global services\.  
To continue receiving global service events outside of US East \(N\. Virginia\), be sure to convert *single\-region trails* using global service events outside of US East \(N\. Virginia\) into *multi\-region trails*\. For more information about capturing global service events, see [Enabling and disabling global service event logging](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses) later in this section\. Also update the region of your `lookup-events` API calls\. For more information about updating lookup events, including an example CLI command, see [Looking up events by attribute](view-cloudtrail-events-cli.md#look-up-events-by-attributes) earlier in this user guide\.

You can use the `update-trail` command to change the configuration settings for a trail\. You can also use the add\-tags and remove\-tags commands to add and remove tags for a trail\. You can only update trails from the AWS Region where the trail was created \(its Home Region\)\. When using the AWS CLI, remember that your commands run in the AWS Region configured for your profile\. If you want to run the commands in a different Region, either change the default Region for your profile, or use the \-\-region parameter with the command\.

**Note**  
If you use the AWS CLI or one of the AWS SDKs to modify a trail, be sure that the trail's bucket policy is up\-to\-date\. In order for your bucket to automatically receive events from a new AWS Region, the policy must contain the full service name, `cloudtrail.amazonaws.com`\. For more information, see [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

**Topics**
+ [Converting a trail that applies to one Region to apply to all Regions](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert)
+ [Converting a multi\-region trail to a single\-region trail](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce)
+ [Enabling and disabling global service event logging](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses)
+ [Enabling log file validation](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi)
+ [Disabling log file validation](#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-lfi-disable)

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

## Enabling and disabling global service event logging<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses"></a>

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

Single\-region trails will no longer receive global service events beginning November 22, 2021, unless the trail already appears in US East \(N\. Virginia\) region, us\-east\-1\. To continue capturing global service events, update the trail configuration to a multi\-region trail\. For example, this command updates a single\-region trail in US East \(Ohio\), us\-east\-2, into a multi\-region trail\. Replace *myExistingSingleRegionTrailWithGSE* with the appropriate trail name for your configuration\.

```
aws cloudtrail --region us-east-2 update-trail --name myExistingSingleRegionTrailWithGSE --is-multi-region-trail
```

Because global service events are only available in US East \(N\. Virginia\) beginning November 22, 2021, you can also create a single\-region trail to subscribe to global service events in the US East \(N\. Virginia\) region, us\-east\-1\. The following command creates a single\-region trail in us\-east\-1 to receive CloudFront, IAM, and AWS STS events:

```
aws cloudtrail --region us-east-1 create-trail --include-global-service-events --name myTrail --s3-bucket-name DOC-EXAMPLE-BUCKET
```

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

To validate log files with the AWS CLI, see [Validating CloudTrail log file integrity with the AWS CLI](cloudtrail-log-file-validation-cli.md)\.