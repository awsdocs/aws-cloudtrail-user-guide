# Creating a Trail for an Organization with the AWS Command Line Interface<a name="cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli"></a>

You can create an organization trail by using the AWS CLI\. The AWS CLI is regularly updated with additional functionality and commands\. To help ensure success, make sure that you have installed or updated to a recent AWS CLI version before you begin\.

**Note**  
The examples in this section are specific to creating and updating organization trails\. For examples of using the AWS CLI to manage trails, see [Managing Trails With the AWS CLI](cloudtrail-additional-cli-commands.md)\. When creating or updating an organization trail with the AWS CLI, you must use an AWS CLI profile in the master account with sufficient permissions\.  
You must configure the Amazon S3 bucket used for an organization trail with sufficient permissions\. 

## Create or update an Amazon S3 bucket to use to store the log files for an organization trail<a name="w37aac10c19c29b7"></a>

You must specify an Amazon S3 bucket to receive the log files for an organization trail\. This bucket must have a policy that allows CloudTrail to put the log files for the organization into the bucket\.

The following is an example policy for an Amazon S3 bucket named *my\-organization\-bucket*\. This bucket is in an AWS account with the ID *111111111111*, which is the master account for an organization with the ID *o\-exampleorgid* that allows logging for an organization trail\. It also allows logging for the *111111111111* account in the event that the trail is changed from an organization trail to a trail for that account only\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailAclCheck20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::my-organization-bucket"
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::my-organization-bucket/AWSLogs/111111111111/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        },
        {
            "Sid": "AWSCloudTrailWrite20150319",
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "cloudtrail.amazonaws.com"
                ]
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::my-organization-bucket/AWSLogs/o-exampleorgid/*",
            "Condition": {
                "StringEquals": {
                    "s3:x-amz-acl": "bucket-owner-full-control"
                }
            }
        }
    ]
}
```

This example policy does not allow any users from member accounts to access the log files created for the organization\. By default, organization log files are accessible only to the master account\. For information about how to allow read access to the Amazon S3 bucket for IAM users in member accounts, see [Sharing CloudTrail Log Files Between AWS Accounts](cloudtrail-sharing-logs.md)\.

## Enabling CloudTrail as a trusted service in AWS Organizations<a name="cloudtrail-create-organization-trail-by-using-the-cli-enable-trusted-service"></a>

Before you can create an organization trail, you must first enable all features in Organizations\. For more information, see [Enabling All Features in Your Organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org_support-all-features.html), or run the following command using a profile with sufficient permissions in the master account:

```
aws organizations enable-all-features
```

After you enable all features, you must configure Organizations to trust CloudTrail as a trusted service\. \.

To create the trusted service relationship between AWS Organizations and CloudTrail, open a terminal or command line and use a profile in the master account\. Run the `aws organizations enable-aws-service-access` command, as demonstrated in the following example\.

```
aws organizations enable-aws-service-access --service-principal cloudtrail.amazonaws.com
```

## Using create\-trail<a name="cloudtrail-create-organization-trail-by-using-the-cli-create-trail"></a>

### Creating an organization trail that applies to all regions<a name="cloudtrail-create-organization-trail-by-using-the-cli-create-trail-all"></a>

To create an organization trail that applies to all regions, use the `--is-organization-trail` and `--is-multi-region-trail` options\. 

**Note**  
When creating or updating an organization trail with the AWS CLI, you must use an AWS CLI profile in the master account with sufficient permissions\.

The following example creates an organization trail that delivers logs from all regions to an existing bucket named `my-bucket`:

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-organization-trail --is-multi-region-trail
```

To confirm that your trail exists in all regions, the `IsOrganizationTrail` and `IsMultiRegionTrail` elements in the output both show `true`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": true,
    "S3BucketName": "my-bucket"
}
```

**Note**  
Use the `start-logging` command to start logging for your trail\. For more information, see [Stopping and starting logging for a trail](cloudtrail-additional-cli-commands.md#cloudtrail-start-stop-logging-cli-commands)\.

### Creating an organization trail as a single\-region trail<a name="cloudtrail-create-organization-trail-by-using-the-cli-single"></a>

The following command creates an organization trail that only logs events in a single AWS Region, also known as a single\-region trail\. The AWS Region where events will be logged is the region specified in the configuration profile for the AWS CLI\.

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-organization-trail
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
    "IsOrganizationTrail": true,
    "S3BucketName": "my-bucket"
}
```

By default, the `create-trail` command creates a single\-region trail and that trail does not enable log file validation\. 

**Note**  
Use the `start-logging` command to start logging for your trail\.

## Using update\-trail to update an organization trail<a name="cloudtrail-update-organization-trail-by-using-the-cli"></a>

You can use the `update-trail` command to change the configuration settings for an organization trail, or to apply an existing trail for a single AWS account to an entire organization\. When doing so, remember that you can run the `update-trail` command only from the region in which the trail was created\.

**Note**  
If you use the AWS CLI or one of the AWS SDKs to modify a trail, be sure that the trail's bucket policy is up\-to\-date\. For more information, see [Creating a Trail for an Organization with the AWS Command Line Interface](#cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli)\.  
When creating or updating an organization trail with the AWS CLI, you must use an AWS CLI profile in the master account with sufficient permissions\.

### Applying an existing trail to an organization<a name="cloudtrail-update-organization-trail-by-using-the-cli-apply-org"></a>

To change an existing trail so that it also applies to an organization instead of a single AWS account, use the `--is-organization-trail` option:

```
aws cloudtrail update-trail --name my-trail --is-organization-trail
```

To confirm that the trail now applies to the organization, the `IsOrganizationTrail` element in the output shows `true`:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "my-trail", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": true, 
    "S3BucketName": "my-bucket"
}
```

In the above example, the trail was previously configured to apply to all regions \(`"IsMultiRegionTrail": true`\)\. If this had been a trail that only applied to a single region, the output would show `"IsMultiRegionTrail": false`\.

### Converting an organization trail that applies to one region to apply to all regions<a name="cloudtrail-update-organization-trail-by-using-the-cli-single-to-all"></a>

To change an existing organization trail so that it applies to all regions use the `--is-multi-region-trail` option:

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
    "IsOrganizationTrail": true,
    "S3BucketName": "my-bucket"
}
```