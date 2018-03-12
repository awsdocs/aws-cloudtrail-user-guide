# Receiving CloudTrail Log Files from Multiple Regions<a name="receive-cloudtrail-log-files-from-multiple-regions"></a>

You can configure CloudTrail to deliver log files from multiple regions to a single S3 bucket for a single account\. For example, you have a trail in the US West \(Oregon\) Region that is configured to deliver log files to a S3 bucket, and a CloudWatch Logs log group\. When you apply the trail to all regions, CloudTrail creates a new trail in all other regions\. This trail has the original trail configuration\. CloudTrail delivers log files to the same S3 bucket and CloudWatch Logs log group\.

**To receive CloudTrail log files from multiple regions**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Trails**, and then choose a trail name\.

1. Click the pencil icon next to **Apply trail to all regions**, and then choose **Yes**\.

1. Choose **Save**\. The original trail is now replicated across all regions\. CloudTrail delivers log files from all regions to the specified S3 bucket\.

**Note**  
When a new region launches in the [aws partition](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html), CloudTrail automatically creates a trail for you in the new region with the same settings as your original trail\.

For more information, see the following resources:

+ [How Does CloudTrail Behave Regionally and Globally?](cloudtrail-concepts.md#cloudtrail-concepts-regional-and-global-services)

+  [CloudTrail FAQs](https://aws.amazon.com/cloudtrail/faqs/) 