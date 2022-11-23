# Amazon S3 bucket policy for CloudTrail Lake query results<a name="s3-bucket-policy-lake-query-results"></a>

By default, Amazon S3 buckets and objects are private\. Only the resource owner \(the AWS account that created the bucket\) can access the bucket and objects it contains\. The resource owner can grant access permissions to other resources and users by writing an access policy\.

To deliver CloudTrail Lake query results to an S3 bucket, CloudTrail must have the required permissions, and it cannot be configured as a [Requester Pays](https://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html) bucket\.

CloudTrail adds the following fields in the policy for you: 
+ The allowed SIDs
+ The bucket name
+ The service principal name for CloudTrail

As a security best practice, add an `aws:SourceArn` condition key to the Amazon S3 bucket policy\. The IAM global condition key `aws:SourceArn` helps ensure that CloudTrail writes to the S3 bucket only for the event data store\. 

The following policy allows CloudTrail to deliver query results to the bucket from supported regions\. Replace *myBucketName*, *myAccountID*, and *myQueryRunningRegion* with the appropriate values for your configuration\. The *myAccountID* is the AWS account ID used for CloudTrail, which may not be the same as the AWS account ID for the S3 bucket\.

**Note**  
If this is an organization event data store, you must use the AWS account ID for the management account\.

**S3 bucket policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailLake1",
            "Effect": "Allow",
            "Principal": {"Service": "cloudtrail.amazonaws.com"},
            "Action": [
                "s3:PutObject*",
                "s3:Abort*"
            ],
            "Resource": [
                "arn:aws:s3:::myBucketName",
                "arn:aws:s3:::myBucketName/*"
            ],
            "Condition": {
                "StringLike": {
                    "aws:sourceAccount": "myAccountID",
                    "aws:sourceArn": "arn:aws:cloudtrail:myQueryRunningRegion:myAccountID:eventdatastore/*"
                }
            }     
        },
        {
            "Sid": "AWSCloudTrailLake2",
            "Effect": "Allow",
            "Principal": {"Service":"cloudtrail.amazonaws.com"},
            "Action": "s3:GetBucketAcl",
            "Resource": "arn:aws:s3:::myBucketName",
            "Condition": {
                "StringLike": {
                    "aws:sourceAccount": "myAccountID",
                    "aws:sourceArn": "arn:aws:cloudtrail:myQueryRunningRegion:myAccountID:eventdatastore/*"
                }
            }
        }
    ]
}
```

**Contents**
+ [Specifying an existing bucket for CloudTrail Lake query results](#specify-an-existing-bucket-for-cloudtrail-query-results-delivery)
+ [Additional resources](#cloudtrail-lake-S3-bucket-policy-resources)

## Specifying an existing bucket for CloudTrail Lake query results<a name="specify-an-existing-bucket-for-cloudtrail-query-results-delivery"></a>

If you specified an existing S3 bucket as the storage location for CloudTrail Lake query results delivery, you must attach a policy to the bucket that allows CloudTrail to deliver the query results to the bucket\.

**Note**  
As a best practice, use a dedicated S3 bucket for CloudTrail Lake query results\.

**To add the required CloudTrail policy to an Amazon S3 bucket**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the bucket where you want CloudTrail to deliver your Lake query results, and then choose **Permissions**\. 

1. Choose **Edit**\.

1. Copy the [S3 bucket policy for query results](#s3-bucket-policy-lake-query) to the **Bucket Policy Editor** window\. Replace the placeholders in italics with the names of your bucket, region, and account ID\.
**Note**  
If the existing bucket already has one or more policies attached, add the statements for CloudTrail access to that policy or policies\. Evaluate the resulting set of permissions to be sure that they are appropriate for the users who access the bucket\.

## Additional resources<a name="cloudtrail-lake-S3-bucket-policy-resources"></a>

For more information about S3 buckets and policies, see [Using bucket policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-policies.html) in the *Amazon Simple Storage Service User Guide*\.