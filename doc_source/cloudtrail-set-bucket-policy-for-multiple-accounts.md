# Setting Bucket Policy for Multiple Accounts<a name="cloudtrail-set-bucket-policy-for-multiple-accounts"></a>

 For a bucket to receive log files from multiple accounts, its bucket policy must grant CloudTrail permission to write log files from all the accounts you specify\. This means that you must modify the bucket policy on your destination bucket to grant CloudTrail permission to write log files from each specified account\.

**To modify bucket permissions so that files can be received from multiple accounts**

1.  Sign in to the AWS Management Console using the account that owns the bucket \(111111111111 in this example\) and open the Amazon S3 console\. 

1. Choose the bucket where CloudTrail delivers your log files and then choose **Properties**\. 

1. Choose **Permissions**\.

1. Choose **Edit Bucket Policy**\.

1.  Modify the existing policy to add a line for each additional account whose log files you want delivered to this bucket\. See the following example policy and note the underlined `Resource` line specifying a second account ID\.
**Note**  
An AWS account ID is a twelve\-digit number, and leading zeros must not be omitted\. 

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "AWSCloudTrailAclCheck20131101",
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudtrail.amazonaws.com"
         },
         "Action": "s3:GetBucketAcl",
         "Resource": "arn:aws:s3:::myBucketName"
       },
       {
         "Sid": "AWSCloudTrailWrite20131101",
         "Effect": "Allow",
         "Principal": {
           "Service": "cloudtrail.amazonaws.com"
         },
         "Action": "s3:PutObject",
         "Resource": [
           "arn:aws:s3:::myBucketName/[optional] myLogFilePrefix/AWSLogs/111111111111/*",
           "arn:aws:s3:::myBucketName/[optional] myLogFilePrefix/AWSLogs/222222222222/*"
         ],
         "Condition": { 
           "StringEquals": { 
             "s3:x-amz-acl": "bucket-owner-full-control" 
           }
         }
       }
     ]
   }
   
   1. Optional restrict access to the bucket to either a specific AWS Organization or Account by utilizing the Condition Context Key [aws:PrincipalOrgID](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#principal-org-id) or [`aws:SourceAccount`](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount)
   ```
