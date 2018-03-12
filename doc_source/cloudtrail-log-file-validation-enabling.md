# Enabling Log File Integrity Validation for CloudTrail<a name="cloudtrail-log-file-validation-enabling"></a>

You can enable log file integrity validation by using the AWS Management Console, AWS Command Line Interface \(AWS CLI\), or CloudTrail API\. CloudTrail starts delivering digest files in about an hour\. 

## AWS Management Console<a name="cloudtrail-log-file-validation-enabling-console"></a>

To enable log file integrity validation with the CloudTrail console, choose **Yes** for the **Enable log file validation** option when you create or update a trail\. By default, this feature is enabled for new trails\. For more information, see [Creating a Trail with the Console](cloudtrail-create-and-update-a-trail-by-using-the-console.md)\. 

## AWS CLI<a name="cloudtrail-log-file-validation-enabling-cli"></a>

To enable log file integrity validation with the AWS CLI, use the `--enable-log-file-validation` option with the [create\-trail](http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/create-trail.html) or [update\-trail](http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/update-trail.html) commands\. To disable log file integrity validation, use the `--no-enable-log-file-validation` option\.

**Example**

The following `update-trail` command enables log file validation and starts delivering digest files to the Amazon S3 bucket for the specified trail\.

```
aws cloudtrail update-trail --name your-trail-name --enable-log-file-validation
```

## CloudTrail API<a name="cloudtrail-log-file-validation-enabling-api"></a>

 To enable log file integrity validation with the CloudTrail API, set the `EnableLogFileValidation` request parameter to `true` when calling `CreateTrail` or `UpdateTrail`\. 

 For more information, see [CreateTrail](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/api_createtrail.html) and [UpdateTrail](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateTrail.html) in the [AWS CloudTrail API Reference](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.