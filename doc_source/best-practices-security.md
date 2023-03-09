# Security best practices in AWS CloudTrail<a name="best-practices-security"></a>

AWS CloudTrail provides a number of security features to consider as you develop and implement your own security policies\. The following best practices are general guidelines and don’t represent a complete security solution\. Because these best practices might not be appropriate or sufficient for your environment, treat them as helpful considerations rather than prescriptions\.

**Topics**
+ [CloudTrail detective security best practices](#best-practices-security-detective)
+ [CloudTrail preventative security best practices](#best-practices-security-preventative)

## CloudTrail detective security best practices<a name="best-practices-security-detective"></a>

**Create a trail**

For an ongoing record of events in your AWS account, you must create a trail\. Although CloudTrail provides 90 days of event history information for management events in the CloudTrail console without creating a trail, it is not a permanent record, and it does not provide information about all possible types of events\. For an ongoing record, and for a record that contains all the event types you specify, you must create a trail, which delivers log files to an Amazon S3 bucket that you specify\. 

To help manage your CloudTrail data, consider creating one trail that logs management events in all AWS Regions, and then creating additional trails that log specific event types for resources, such as Amazon S3 bucket activity or AWS Lambda functions\.

The following are some steps you can take:
+ [Create a trail for your AWS account\.](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console)
+ [Create a trail for an organization\.](creating-trail-organization.md)

**Apply trails to all AWS Regions**

To obtain a complete record of events taken by an IAM identity, or service in your AWS account, each trail should be configured to log events in all AWS Regions\. By logging events in all AWS Regions, you ensure that all events that occur in your AWS account are logged, regardless of which AWS Region where they occurred\. This includes logging [global service events](cloudtrail-concepts.md#cloudtrail-concepts-global-service-events), which are logged to an AWS Region specific to that service\. When you create a trail that applies to all regions, CloudTrail records events in each region and delivers the CloudTrail event log files to an S3 bucket that you specify\. If an AWS Region is added after you create a trail that applies to all regions, that new region is automatically included, and events in that region are logged\. This is the default option when you create a trail in the CloudTrail console\. 

The following are some steps you can take:
+ [Create a trail for your AWS account\.](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console)
+ [Update an existing trail ](cloudtrail-update-a-trail-console.md)to log events in all AWS Regions\.
+ Implement ongoing detective controls to help ensure all trails created are logging events in all AWS Regions by using the [multi\-region\-cloud\-trail\-enabled](https://docs.aws.amazon.com/config/latest/developerguide/multi-region-cloudtrail-enabled.html) rule in AWS Config\.

**Enable CloudTrail log file integrity**

Validated log files are especially valuable in security and forensic investigations\. For example, a validated log file enables you to assert positively that the log file itself has not changed, or that particular IAM identity credentials performed specific API activity\. The CloudTrail log file integrity validation process also lets you know if a log file has been deleted or changed, or assert positively that no log files were delivered to your account during a given period of time\. CloudTrail log file integrity validation uses industry standard algorithms: SHA\-256 for hashing and SHA\-256 with RSA for digital signing\. This makes it computationally unfeasible to modify, delete or forge CloudTrail log files without detection\. For more information, see [Enabling validation and validating files](cloudtrail-log-file-validation-intro.md#cloudtrail-log-file-validation-intro-enabling-and-using)\.

**Integrate with Amazon CloudWatch Logs**

CloudWatch Logs allows you to monitor and receive alerts for specific events captured by CloudTrail\. The events sent to CloudWatch Logs are those configured to be logged by your trail, so make sure you have configured your trail or trails to log the event types \(management events and/or data events\) that you are interested in monitoring\.

For example, you can monitor key security and network\-related management events, such as [failed AWS Management Console sign\-in events](cloudwatch-alarms-for-cloudtrail.md#cloudwatch-alarms-for-cloudtrail-signin)\.

The following are some steps you can take:
+ Review example [CloudWatch Logs integrations for CloudTrail](cloudwatch-alarms-for-cloudtrail.md)\.
+ Configure your trail to [send events to CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.
+ Consider implementing ongoing detective controls to help ensure all trails are sending events to CloudWatch Logs for monitoring by using the [cloud\-trail\-cloud\-watch\-logs\-enabled](https://docs.aws.amazon.com/config/latest/developerguide/cloud-trail-cloud-watch-logs-enabled.html) rule in AWS Config\. 

**Use AWS Security Hub**

Monitor your usage of CloudTrail as it relates to security best practices by using [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html)\. Security Hub uses detective *security controls* to evaluate resource configurations and *security standards* to help you comply with various compliance frameworks\. For more information about using Security Hub to evaluate CloudTrail resources, see [AWS CloudTrail controls](https://docs.aws.amazon.com/securityhub/latest/userguide/cloudtrail-controls.html) in the *AWS Security Hub User Guide*\.

## CloudTrail preventative security best practices<a name="best-practices-security-preventative"></a>

The following best practices for CloudTrail can help prevent security incidents\.

**Log to a dedicated and centralized Amazon S3 bucket**

CloudTrail log files are an audit log of actions taken by an IAM identity or an AWS service\. The integrity, completeness and availability of these logs is crucial for forensic and auditing purposes\. By logging to a dedicated and centralized Amazon S3 bucket, you can enforce strict security controls, access, and segregation of duties\. 

The following are some steps you can take:
+ Create a separate AWS account as a log archive account\. If you use AWS Organizations, enroll this account in the organization, and consider [creating an organization trail](creating-trail-organization.md) to log data for all AWS accounts in your organization\.
+ If you do not use Organizations but want to log data for multiple AWS accounts, [create a trail](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console) to log activity in this log archive account\. Restrict access to this account to only trusted administrative users who should have access to account and auditing data\.
+ As part of creating a trail, whether it is an organization trail or a trail for a single AWS account, create a dedicated Amazon S3 bucket to store log files for this trail\. 
+ If you want to log activity for more than one AWS account, [modify the bucket policy](cloudtrail-set-bucket-policy-for-multiple-accounts.md) to allow logging and storing log files for all AWS accounts that you want to log AWS account activity\.
+ If you are not using an organization trail, create trails in all of your AWS accounts, specifying the Amazon S3 bucket in the log archive account\.

**Use server\-side encryption with AWS KMS managed keys**

By default, the log files delivered by CloudTrail to your bucket are encrypted by Amazon [server\-side encryption with Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\. To provide a security layer that is directly manageable, you can instead use [server\-side encryption with AWS KMS–managed keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) for your CloudTrail log files\. To use SSE\-KMS with CloudTrail, you create and manage an [AWS KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html), also known as a KMS key\. 

**Note**  
If you use SSE\-KMS and log file validation, and you have modified your Amazon S3 bucket policy to only allow SSE\-KMS encrypted files, you will not be able to create trails that utilize that bucket unless you modify your bucket policy to specifically allow AES256 encryption, as shown in the following example policy line\.  

```
"StringNotEquals": { "s3:x-amz-server-side-encryption": ["aws:kms", "AES256"] } 
```

The following are some steps you can take:
+ [Review the advantages of encrypting your log files with SSE\-KMS](encrypting-cloudtrail-log-files-with-aws-kms.md)\.
+ [Create a KMS key to use for encrypting log files](create-kms-key-policy-for-cloudtrail.md)\.
+ [Configure log file encryption for your trails\.](create-kms-key-policy-for-cloudtrail-update-trail.md)
+ Consider implementing ongoing detective controls to help ensure all trails are encrypting log files with SSE\-KMS by using the [cloud\-trail\-encryption\-enabled](https://docs.aws.amazon.com/config/latest/developerguide/cloud-trail-encryption-enabled.html) rule in AWS Config\. 

**Add a condition key to the default Amazon SNS topic policy**

When you configure a trail to send notifications to Amazon SNS, CloudTrail adds a policy statement to your SNS topic access policy that allows CloudTrail to send content to an SNS topic\. As a security best practice, we recommend adding an `aws:SourceArn` \(or optionally `aws:SourceAccount`\) condition key to the CloudTrail policy statement\. This helps prevent unauthorized account access to your SNS topic\. For more information, see [Amazon SNS topic policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)\.

**Implement least privilege access to Amazon S3 buckets where you store log files**

CloudTrail trails log events to an Amazon S3 bucket that you specify\. These log files contain an audit log of actions taken by IAM identities and AWS services\. The integrity and completeness of these log files are crucial for auditing and forensic purposes\. In order to help ensure that integrity, you should adhere to the principle of least privilege when creating or modifying access to any Amazon S3 bucket used for storing CloudTrail log files\. 

Take the following steps:
+ Review the [Amazon S3 bucket policy](create-s3-bucket-policy-for-cloudtrail.md) for any and all buckets where you store log files and adjust it if necessary to remove any unnecessary access\. This bucket policy will be generated for you if you create a trail using the CloudTrail console, but can also be created and managed manually\.
+ As a security best practice, be sure to manually add a `aws:SourceArn` condition key to the bucket policy\. For more information, see [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.
+ If you are using the same Amazon S3 bucket to store log files for multiple AWS accounts, follow the guidance for [receiving log files for multiple accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.
+ If you are using an organization trail, make sure you follow the guidance for [organization trails](creating-trail-organization.md), and review the example policy for an Amazon S3 bucket for an organization trail in [Creating a trail for an organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.
+ Review the [Amazon S3 security documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/security.html) and the [example walkthrough for securing a bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/walkthrough1.html)\.

**Enable MFA Delete on the Amazon S3 bucket where you store log files**

When you configure multi\-factor authentication \(MFA\), attempts to change the versioning state of bucket, or delete an object version in a bucket, require additional authentication\. This way, even if a user acquires the password of an IAM user with permissions to permanently delete Amazon S3 objects, you can still prevent operations that could compromise your log files\.

The following are some steps you can take:
+ Review the [MFA Delete](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html#MultiFactorAutenticationDelete) guidance\.
+ [Add an Amazon S3 bucket policy to require MFA](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html#example-bucket-policies-use-case-7)\.

**Configure object lifecycle management on the Amazon S3 bucket where you store log files**

The CloudTrail trail default is to store log files indefinitely in the Amazon S3 bucket configured for the trail\. You can use the [Amazon S3 object lifecycle management rules](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) to define your own retention policy to better meet your business and auditing needs\. For example, you might want to archive log files that are more than a year old to Amazon Glacier, or delete log files after a certain amount of time has passed\.

**Limit access to the AWSCloudTrail\_FullAccess policy**

Users with the [AWSCloudTrail\_FullAccess](security_iam_id-based-policy-examples.md#grant-custom-permissions-for-cloudtrail-users-full-access) policy have the ability to disable or reconfigure the most sensitive and important auditing functions in their AWS accounts\. This policy is not intended to be shared or applied broadly to IAM identities in your AWS account\. Limit application of this policy to as few individuals as possible, those you expect to act as AWS account administrators\.