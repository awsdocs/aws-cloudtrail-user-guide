# Deleting a Trail<a name="cloudtrail-delete-trails-console"></a>

 You can delete trails with the CloudTrail console\. If you want to delete a trail that receives log files from all regions, you must choose the region where you originally created the trail\.

**To delete a trail with the CloudTrail console**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Navigate to the **Trails** page of the CloudTrail console for the region in which the trail was created\.

1. Choose the trail name\.

1. At the top of the configuration page, choose the trash icon\.

1. Choose **Delete** to delete the trail permanently\. The trail is removed from the list of trails for the region\. Log files that were already delivered to the Amazon S3 bucket are not deleted\.
**Note**  
Content delivered to Amazon S3 buckets might contain customer content\. For more information about removing sensitive data, see [How Do I Empty an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/empty-bucket.html) or [How Do I Delete an S3 Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/delete-bucket.html)\.