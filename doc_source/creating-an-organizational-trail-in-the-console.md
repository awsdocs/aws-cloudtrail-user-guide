# Creating a Trail For Your Organization in the Console<a name="creating-an-organizational-trail-in-the-console"></a>

 To create an organization trail in the CloudTrail console, you must sign in to the console using an IAM user or role in the master account with [sufficient permissions](creating-an-organizational-trail-prepare.md#org_trail_permissions)\. If you are not signed in with the master account, you will not see the option to apply a trail to an organization when you create or edit a trail in the CloudTrail console\. 

You can choose to configure an organization trail in various ways\. For example, you can: 
+ Specify if you want the trail to apply to all regions or a single region\. The default is to create a trail for all regions\. For more information, see [How CloudTrail Works](how-cloudtrail-works.md)\.
+ Specify whether to apply the trail to your organization\. The default is not to do so; you must choose this option in order to create an organization trail\.
+ Specify which Amazon S3 bucket to use to receive log files for the organization trail\. You can choose an existing Amazon S3 bucket in the master account, or create one specifically for the organization trail\. 
+ For management and data events, specify if you want to log read\-only, write\-only, or all events\. You can specify logging data events for specific Amazon S3 buckets and AWS Lambda functions in the master account by choosing them from the lists in the console, and in member accounts if you specify the ARNs of each resource for which you want to enable data event logging\. For more information, see [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.

**To create an organization trail with the AWS Management Console**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

   You must be signed in as a user, role, or root account in the master account with [sufficient permissions](creating-an-organizational-trail-prepare.md#org_trail_permissions) to create an organization trail\.

1. In the region selector, choose the region where you want the trail to be created\. 

1. Choose **Trails**, and then choose **Create trail**\.
**Tip**  
If you see **Get Started Now** instead of **Trails**, choose it\.

1. On the **Create Trail** page, for **Trail name**, type a name for your organization trail\. This name must be unique in the master account and in all member accounts\. Keep in mind that this trail name will appear in all member accounts in the CloudTrail console for those accounts, as well as in the list of trails provided by the AWS CLI and API\. Consider providing an easily understood name for this trail that meets the naming requirements\. For more information, see [CloudTrail Trail Naming Requirements](cloudtrail-trail-naming-requirements.md)\.

1. For **Apply trail to all regions**, choose **Yes** to receive log files from all regions\. This is the default and recommended setting\. If you choose **No**, the trail logs files only from the region in which you create the trail\.

1. For **Apply trail to my organization**, choose **Yes**\. You will only see this option if you are signed in to the console with an IAM user or role in the master account\. In order to successfully create an organization trail, make sure that the user or role has [sufficient permissions](creating-an-organizational-trail-prepare.md#org_trail_permissions)\. 

1. For **Management events**, for **Read/Write events**, choose if you want your trail to log **All**, **Read\-only**, **Write\-only**, or **None**\. By default, trails log all management events\. For more information, see [Management Events](logging-management-and-data-events-with-cloudtrail.md#logging-management-events)\.

1. For **Data events**, you can specify logging data events for Amazon S3 buckets, for AWS Lambda functions\. You can choose from the lists of specific Amazon S3 buckets and Lambda functions in the master account, but these lists do not include any resource information for member accounts\. To add specific resources in member accounts, you must provide the ARNs of Amazon S3 buckets and Lambda functions\. Alternatively, you can select the option to log all Amazon S3 buckets and Lambda functions\. This choice applies to all accounts in the organization\. 

   By default, trails don't log data events\. Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. 

   For Amazon S3 buckets:
   + Choose the **S3** tab\.
   + To specify a bucket in the master account, choose **Add S3 bucket**\. Either choose the bucket from the list that appears when you choose *Bucket name*, orype the S3 bucket name and prefix \(optional\) for which you want to log data events\. For each bucket, specify whether you want to log **Read** events, such as `GetObject`, **Write** events, such as `PutObject`, or both\. For more information, see [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.
   + To specify a bucket in a member account, choose **Add S3 bucket**\. Type or paste the ARN for that bucket into *Bucket name*\. For each bucket, specify whether you want to log **Read** events, such as `GetObject`, **Write** events, such as `PutObject`, or both\. For more information, see [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.
   + To log data events for all S3 buckets in all accounts in your organization, select **Select all S3 buckets in your account**\. Then choose whether you want to log **Read** events, such as `GetObject`, **Write** events, such as `PutObject`, or both\. This setting takes precedence over individual settings you configure for individual buckets in master and member accounts\. For example, if you specify logging **Read** events for all S3 buckets, and then choose to add a specific bucket for data event logging, **Read** is already selected for the bucket you added\. You cannot clear the selection\. You can only configure the option for **Write**\. 
**Note**  
Selecting the **Select all S3 buckets in your account** option enables data event logging for all buckets in all accounts currently in your organization and any buckets created in the master and member accounts after you finish creating the trail\. It also enables logging of data event activity performed by any user or role in your master account, even if that activity is performed on a bucket that belongs to another AWS account outside of your organization\.  
If the trail applies only to one region, selecting the **Select all S3 buckets in your account** option enables data event logging for all buckets in the organization in the same region as your trail and any buckets created in the master and member accounts in that region after you enable the option\. It will not log data events for Amazon S3 buckets in other regions in your organization\.

   For Lambda functions:
   + Choose the **Lambda** tab\.
   + To specify logging data events for individual functions in your master account, select them from the list\. Only functions in the master account are listed\. Functions in member accounts do not appear in the list\.
**Note**  
If you have more than 15,000 Lambda functions in your master account, you cannot view or select all functions in the CloudTrail console when creating a trail\. You can still select the option to log all functions, even if they are not displayed\. If you want to log data events for specific functions, you can manually add a function if you know its ARN\. You can also finish creating the trail in the console, and then use the AWS CLI and the put\-event\-selectors command to configure data event logging for specific Lambda functions\. For more information, see [Managing Trails](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md#cloudtrail-additional-cli-commands)\.
   + To specify logging data events for individual functions in member accounts, choose **Add function**, and then type or paste the ARN of the function in the member account\.
   + To log data events for all Lambda functions in your organization, select **Log all current and future functions**\. This setting takes precedence over individual settings you configure for individual functions\. All functions are logged, even if all functions are not displayed\.
**Note**  
If you are creating a trail for all regions, this selection enables data event logging for all functions currently in your organization, and any Lambda functions that might be created in the master and member accounts in any region after you finish creating the trail\. If you are creating a trail for a single region, this selection enables data event logging for all functions currently in that region in all accounts in your organization, and any Lambda functions created in that region in the master and member accounts after you finish creating the trail\. It does not enable data event logging for Lambda functions created in other regions\.  
Logging data events for all functions also enables logging of data event activity performed by any user or role in your master account, even if that activity is performed on a function that belongs to another AWS account outside of the organization\.

1. For **Storage location**, for **Create a new S3 bucket**, choose **Yes** to create a bucket\. When you create a bucket, CloudTrail creates and applies the required bucket policies for an organization trail\.
**Note**  
If you choose **No**, choose an existing Amazon S3 bucket\. The bucket policy will be updated to allow for logging for an organization trail\. For information about manually creating or editing the bucket policy, see [Creating a Trail for an Organization with the AWS Command Line Interface](cloudtrail-create-and-update-an-organizational-trail-by-using-the-aws-cli.md)\.

1. For **S3 bucket**, type a name for the bucket you want to designate for log file storage\. The name must be globally unique\. For more information, see [Amazon S3 Bucket Naming Requirements](cloudtrail-s3-bucket-naming-requirements.md)\.

1. To configure advanced settings, see [Configuring Advanced Settings for Your Organization Trail](#advanced-settings-for-your-organizational-trail)\. Otherwise, choose **Create**\.

1. The new trail appears on the **Trails** page\. An organization trail might take up to 24 hours to be created in all regions in all member accounts\. The **Trails** page shows the trails in your account from all regions\. In about 15 minutes, CloudTrail publishes log files that show the AWS API calls made in your organization\. You can see the log files in the Amazon S3 bucket that you specified\.

**Note**  
You can't rename a trail after it has been created\. Instead, you can delete the trail and create a new one\. 

## Configuring Advanced Settings for Your Organization Trail<a name="advanced-settings-for-your-organizational-trail"></a>

You can configure the following settings for your organization trail:
+ Specify a log file prefix for the Amazon S3 bucket receiving log files\.
+ Encrypt log files with AWS Key Management Service \(SSE\-KMS\) instead of the default encryption \([Amazon S3\-managed encryption keys \(SSE\-S3\)\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\.
+ Enable log file validation for logs\.
+ Configure Amazon SNS to notify you when log files are delivered\.

**To configure advanced settings for your trail**

1. For **Storage location**, choose **Advanced**\.

1. In the **Log file prefix** field, type a prefix for your Amazon S3 bucket\. The prefix is an addition to the URL for an Amazon S3 object that creates a folder\-like organization in your bucket\. The location where your log files will be stored appears under the text field\.

1. For **Encrypt log files with SSE\-KMS**, choose **Yes** if you want to encrypt your log files with SSE\-KMS instead of SSE\-S3\.

1. For **Create a new KMS key**, choose **Yes** to create a key in the master account or **No** to use an existing one in the master account\.

1. If you chose **Yes**, in the **KMS key** field, type an alias\. CloudTrail encrypts your log files with the key and adds the policy for you\.
**Note**  
If you choose **No**, choose an existing KMS key\. You can also type the ARN of a key from another account\. For more information, see [Updating a Trail to Use Your CMK](create-kms-key-policy-for-cloudtrail-update-trail.md)\. The key policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\. For information about manually editing the key policy, see [AWS KMS Key Policy for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

1. For **Enable log file validation**, choose **Yes** to have log digests delivered to your Amazon S3 bucket\. You can use the digest files to verify that your log files did not change after CloudTrail delivered them\. For more information, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\. 

1. For **Send SNS notification for every log file delivery**, choose **Yes** if you want to be notified each time a log is delivered to your bucket\. CloudTrail stores multiple events in a log file\. SNS notifications are sent for every log file, not for every event\. 

1. For **Create a new SNS topic**, choose **Yes** to create a topic, or choose **No** to use an existing topic\. If you are creating a trail that applies to all regions, SNS notifications for log file deliveries from all regions are sent to the single SNS topic that you create\.
**Note**  
If you choose **No**, choose an existing topic\. You can also enter the ARN of a topic from another region or from an account with appropriate permissions\. For more information, see [Amazon SNS Topic Policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)\.

1. If you choose **Yes**, in the **SNS topic** field, type a name\.

   If you create a topic, you must subscribe to the topic to be notified of log file delivery\. You can subscribe from the Amazon SNS console\. Due to the frequency of notifications, we recommend that you configure the subscription to use an Amazon SQS queue to handle notifications programmatically\. For more information, see the [Amazon Simple Notification Service Getting Started Guide](https://docs.aws.amazon.com/sns/latest/gsg/)\.

1. Choose **Create**\.

## Next Steps<a name="cloudtrail-create-an-organizational-trail-using-the-console-first-time-next-steps"></a>

After you create your trail, you can return to the trail to make changes:
+ Change the configuration of your trail by editing it\. For more information, see [Updating a Trail](cloudtrail-update-a-trail-console.md)\.
+ Configure the Amazon S3 bucket to allow specific IAM users in member accounts to read the log files for the organization, if desired\. For more information, see [Sharing CloudTrail Log Files Between AWS Accounts](cloudtrail-sharing-logs.md)\.
+ Configure CloudTrail to send log files to CloudWatch Logs\. For more information, see [Sending Events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md) and [the CloudWatch Logs item](creating-an-organizational-trail-prepare.md#cwl-org-pb) in [Prepare For Creating a Trail For Your Organization](creating-an-organizational-trail-prepare.md)\.
+ Create a table and use it to run a query in Amazon Athena to analyze your AWS service activity\. For more information, see [Creating a Table for CloudTrail Logs in the CloudTrail Console](https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html#create-cloudtrail-table-ct) in the [Amazon Athena User Guide](https://docs.aws.amazon.com/athena/latest/ug/)\.
+ Add custom tags \(key\-value pairs\) to the trail\.
+ To create another organization trail, return to the **Trails** page and choose **Create trail**\.

**Note**  
When configuring a trail, you can choose an Amazon S3 bucket and SNS topic that belong to another account\. However, if you want CloudTrail to deliver events to a CloudWatch Logs log group, you must choose a log group that exists in your current account\.