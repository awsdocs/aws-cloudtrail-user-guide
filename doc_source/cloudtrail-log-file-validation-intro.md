# Validating CloudTrail Log File Integrity<a name="cloudtrail-log-file-validation-intro"></a>

 To determine whether a log file was modified, deleted, or unchanged after CloudTrail delivered it, you can use CloudTrail log file integrity validation\. This feature is built using industry standard algorithms: SHA\-256 for hashing and SHA\-256 with RSA for digital signing\. This makes it computationally infeasible to modify, delete or forge CloudTrail log files without detection\. You can use the AWS CLI to validate the files in the location where CloudTrail delivered them\. 

## Why Use It?<a name="cloudtrail-log-file-validation-intro-use-cases"></a>

 Validated log files are invaluable in security and forensic investigations\. For example, a validated log file enables you to assert positively that the log file itself has not changed, or that particular user credentials performed specific API activity\. The CloudTrail log file integrity validation process also lets you know if a log file has been deleted or changed, or assert positively that no log files were delivered to your account during a given period of time\. 

## How It Works<a name="cloudtrail-log-file-validation-intro-how-it-works"></a>

When you enable log file integrity validation, CloudTrail creates a hash for every log file that it delivers\. Every hour, CloudTrail also creates and delivers a file that references the log files for the last hour and contains a hash of each\. This file is called a digest file\. CloudTrail signs each digest file using the private key of a public and private key pair\. After delivery, you can use the public key to validate the digest file\. CloudTrail uses different key pairs for each AWS region\. 

 The digest files are delivered to the same Amazon S3 bucket associated with your trail as your CloudTrail log files\. If your log files are delivered from all regions or from multiple accounts into a single Amazon S3 bucket, CloudTrail will deliver the digest files from those regions and accounts into the same bucket\. 

 The digest files are put into a folder separate from the log files\. This separation of digest files and log files enables you to enforce granular security policies and permits existing log processing solutions to continue to operate without modification\. Each digest file also contains the digital signature of the previous digest file if one exists\. The signature for the current digest file is in the metadata properties of the digest file Amazon S3 object\. For more information about digest file contents, see [CloudTrail Digest File Structure](cloudtrail-log-file-validation-digest-file-structure.md)\.

### Storing log and digest files<a name="cloudtrail-log-file-validation-intro-storage"></a>

 You can store the CloudTrail log files and digest files in Amazon S3 or Amazon Glacier securely, durably and inexpensively for an indefinite period of time\. To enhance the security of the digest files stored in Amazon S3, you can use [Amazon S3 MFA Delete](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMFADelete.html)\. 

### Enabling Validation and Validating Files<a name="cloudtrail-log-file-validation-intro-enabling-and-using"></a>

To enable log file integrity validation, you can use the AWS Management Console, the AWS CLI, or CloudTrail API\. For more information, see [Enabling Log File Integrity Validation for CloudTrail](cloudtrail-log-file-validation-enabling.md)\. 

To validate the integrity of CloudTrail log files, you can use the AWS CLI or create your own solution\. The AWS CLI will validate files in the location where CloudTrail delivered them\. If you want to validate logs that you have moved to a different location, either in Amazon S3 or elsewhere, you can create your own validation tools\. 

For information on validating logs by using the AWS CLI, see [Validating CloudTrail Log File Integrity with the AWS CLI](cloudtrail-log-file-validation-cli.md)\. For information on developing custom implementations of CloudTrail log file validation, see [Custom Implementations of CloudTrail Log File Integrity Validation ](cloudtrail-log-file-custom-validation.md)\. 