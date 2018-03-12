# Overview for Creating a Trail<a name="cloudtrail-create-and-update-a-trail"></a>

You can configure the following settings when you create or update a trail with the CloudTrail console or the AWS Command Line Interface \(AWS CLI\)\. Both methods follow the same steps: 

1. Turn on CloudTrail by creating a trail\. By default, when you create a trail in a region in the CloudTrail console, the trail applies to all regions\.

1. Create an Amazon S3 bucket or specify an existing bucket where you want the log files delivered\. By default, log files from all regions in your account are delivered to the bucket that you specify\.

1. Configure your trail to log read\-only, write\-only, or all management and data events\. By default, trails log all management events\.

1. Create an Amazon SNS topic to receive notifications when log files are delivered\. Delivery notifications from all regions are sent to the topic that you specify\.

1. Configure CloudWatch Logs to receive your logs from CloudTrail so that you can monitor for specific log events\. 

1. Turn on log file encryption\. This encrypts your files for added security\.

1. Turn on integrity validation for log files\. This enables the delivery of digest files that you can use to validate the integrity of log files after CloudTrail has delivered them\.

1. Add tags \(custom key\-value pairs\) to your trail\.


+ [Creating a Trail with the Console](cloudtrail-create-and-update-a-trail-by-using-the-console.md)
+ [Creating a Trail with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)
+ [CloudTrail Trail Naming Requirements](cloudtrail-trail-naming-requirements.md)
+ [Amazon S3 Bucket Naming Requirements](cloudtrail-s3-bucket-naming-requirements.md)
+ [Amazon S3 Bucket Policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)
+ [AWS KMS Alias Naming Requirements](KMS-key-naming-requirements.md)
+ [Tips for Managing Trails](cloudtrail-concepts-trails-managing-and-using.md)