# Data protection in AWS CloudTrail<a name="data-protection"></a>

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS CloudTrail\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with CloudTrail or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

By default, CloudTrail event log files are encrypted using Amazon S3 server\-side encryption \(SSE\)\. You can also choose to encrypt your log files with an AWS Key Management Service \(AWS KMS\) key\. You can store your log files in your bucket for as long as you want\. You can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. If you want notifications about log file delivery and validation, you can set up Amazon SNS notifications\.

The following security best practices also address data protection in CloudTrail:
+ [Encrypting CloudTrail log files with AWS KMS–managed keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)
+ [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)
+ [Validating CloudTrail log file integrity](cloudtrail-log-file-validation-intro.md)
+ [Sharing CloudTrail log files between AWS accounts](cloudtrail-sharing-logs.md)

Because CloudTrail logs files are stored in a bucket or buckets in Amazon S3, you should also review the data protection information in the Amazon Simple Storage Service User Guide\. For more information, see [Protecting Data in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/DataDurability.html)\.