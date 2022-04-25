# Tips for managing trails<a name="cloudtrail-concepts-trails-managing-and-using"></a>
+ Beginning on April 12, 2019, trails will be viewable only in the AWS Regions where they log events\. If you create a trail that logs events in all AWS Regions, it will appear in the console in all AWS Regions\. If you create a trail that only logs events in a single AWS Region, you can view and manage it only in that AWS Region\.
+ To edit a trail in the list, choose the trail name\. 
+ Configure at least one trail that applies to all regions so that you receive log files from all regions in your account\.
+ To log events from a specific region and deliver log files to an S3 bucket in the same region, you can update the trail to apply to a single region\. This is useful if you want to keep your log files separate\. For example, you may want users to manage their own logs in specific regions, or you may want to separate CloudWatch Logs alarms by region\.
+ To log events from multiple AWS accounts in one trail, consider creating an organization in AWS Organizations and then creating an organization trail\.
+ Creating multiple trails will incur additional costs\. For more information about prices, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. 

**Topics**
+ [Managing CloudTrail costs](cloudtrail-trail-manage-costs.md)
+ [CloudTrail trail naming requirements](cloudtrail-trail-naming-requirements.md)
+ [Amazon S3 bucket naming requirements](cloudtrail-s3-bucket-naming-requirements.md)
+ [AWS KMS alias naming requirements](KMS-key-naming-requirements.md)