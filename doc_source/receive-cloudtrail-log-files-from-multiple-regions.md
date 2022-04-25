# Receiving CloudTrail log files from multiple regions<a name="receive-cloudtrail-log-files-from-multiple-regions"></a>

You can configure CloudTrail to deliver log files from multiple regions to a single S3 bucket for a single account\. For example, you have a trail in the US West \(Oregon\) Region that is configured to deliver log files to a S3 bucket, and a CloudWatch Logs log group\. When you change an existing single\-region trail to log all regions, CloudTrail logs events from all regions that are in a single AWS partition in your account\. CloudTrail delivers log files to the same S3 bucket and CloudWatch Logs log group\. As long as CloudTrail has permissions to write to an S3 bucket, the bucket for a multi\-region trail does not have to be in the trail's home region\.

To log events across all regions in all AWS partitions in your account, create a multi\-region trail in each partition\.

In the console, by default, you create a trail that logs events in all AWS Regions\. This is a recommended best practice\. To log events in a single region \(not recommended\), [use the AWS CLI](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single)\. To configure an existing single\-region trail to log in all regions, you must use the AWS CLI\.

To change an existing trail so that it applies to all Regions, add the `--is-multi-region-trail` option to the [update\-trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md) command\.

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

**Note**  
When a new region launches in the [`aws` partition](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html), CloudTrail automatically creates a trail for you in the new region with the same settings as your original trail\.

For more information, see the following resources:
+ [How does CloudTrail behave regionally and globally?](cloudtrail-concepts.md#cloudtrail-concepts-regional-and-global-services)
+  [CloudTrail FAQs](https://aws.amazon.com/cloudtrail/faqs/) 