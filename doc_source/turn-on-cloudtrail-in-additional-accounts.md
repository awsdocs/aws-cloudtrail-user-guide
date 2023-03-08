# Turning on CloudTrail in additional accounts<a name="turn-on-cloudtrail-in-additional-accounts"></a>

You can use the console or the command line interface to turn on CloudTrail in additional AWS accounts\.

## Using the console to turn on CloudTrail in additional AWS accounts<a name="turn-on-cloudtrail-in-additional-accounts-console"></a>

You can use the CloudTrail console to turn on CloudTrail in additional accounts\.

1. Sign into the AWS management console using account 222222222222 credentials and open the AWS CloudTrail console\. In the navigation bar, select the region where you want to turn on CloudTrail\.

1. On the CloudTrail service home page, the **Trails** page, or the **Trails** section of the **Dashboard** page, choose **Create trail**\.

1. On the **Create Trail** page, for **Trail name**, type a name for your trail\. For more information, see [CloudTrail trail naming requirements](cloudtrail-trail-naming-requirements.md)\.

1. If this is an AWS Organizations organization trail, you can choose to enable the trail for all accounts in your organization\. You only see this option if you sign  in to the console with a user or role in the management account or a delegated administrator account\. To create an organization trail, the user or role must have [sufficient permissions](creating-an-organizational-trail-prepare.md#org_trail_permissions)\. For more information, see [Creating a trail for an organization](creating-trail-organization.md)\.

1. For **Storage location**, choose **Use existing S3 bucket**\. Use the text box to enter the name of the bucket you created previously for storing log files when you signed in using account 111111111111 credentials\.
**Note**  
The bucket policy must grant CloudTrail permission to write to it\. For information about manually editing the bucket policy, see [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

   To make it easier to find your logs, create a new folder \(also known as a *prefix*\) in an existing bucket to store your CloudTrail logs\. Enter the prefix in **Prefix**\.

1. For **Prefix**, enter the same prefix you entered for storing log files when you turned on CloudTrail using account 111111111111 credentials\. If you choose to use a prefix that is different from the one you entered when you turned on CloudTrail in the first account, you must edit the bucket policy on your destination bucket to allow CloudTrail to write log files to your bucket using this new prefix\.

1. For **Log file SSE\-KMS encryption**, choose **Enabled** if you want to encrypt your log files using SSE\-KMS encryption instead of SSE\-S3 encryption\. The default is **Enabled**\. If you don't enable SSE\-KMS encryption, your logs are encrypted using SSE\-S3 encryption\. For more information about SSE\-KMS encryption, see [Using server\-side encryption with AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)\. For more information about SSE\-S3 encryption, see [Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html)\.

   If you enable SSE\-KMS encryption, choose a **New** or **Existing** AWS KMS key\. In **AWS KMS Alias**, specify an alias, in the format `alias/`*MyAliasName*\. For more information, see [Updating a resource to use your KMS key](create-kms-key-policy-for-cloudtrail-update-trail.md)\. CloudTrail also supports AWS KMS multi\-Region keys\. For more information about multi\-Region keys, see [Using multi\-Region keys](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
You can also type the ARN of a key from another account\. The key policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\. For information about manually editing the key policy, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\. 

1. In **Additional settings**, configure the following\.

   1. For **Log file validation**, choose **Enabled** to have log digests delivered to your S3 bucket\. You can use the digest files to verify that your log files did not change after CloudTrail delivered them\. For more information, see [Validating CloudTrail log file integrity](cloudtrail-log-file-validation-intro.md)\.

   1. For **SNS notification delivery**, choose **Enabled** to be notified each time a log is delivered to your bucket\. CloudTrail stores multiple events in a log file\. SNS notifications are sent for every log file, not for every event\. For more information, see [Configuring Amazon SNS notifications for CloudTrail](configure-sns-notifications-for-cloudtrail.md)\.

      If you enable SNS notifications, for **Create a new SNS topic**, choose **New** to create a topic, or choose **Existing** to use an existing topic\. If you are creating a trail that applies to all Regions, SNS notifications for log file deliveries from all Regions are sent to the single SNS topic that you create\.

      If you choose **New**, CloudTrail specifies a name for the new topic for you, or you can type a name\. If you choose **Existing**, choose an SNS topic from the drop\-down list\. You can also enter the ARN of a topic from another Region or from an account with appropriate permissions\. For more information, see [Amazon SNS topic policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)\.

      If you create a topic, you must subscribe to the topic to be notified of log file delivery\. You can subscribe from the Amazon SNS console\. Due to the frequency of notifications, we recommend that you configure the subscription to use an Amazon SQS queue to handle notifications programmatically\. For more information, see the [Amazon Simple Notification Service Getting Started Guide](https://docs.aws.amazon.com/sns/latest/gsg/)\.

1. Optionally, configure CloudTrail to send log files to CloudWatch Logs by choosing **Enabled** in **CloudWatch Logs**\. For more information, see [Sending events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\.

   1. If you enable integration with CloudWatch Logs, choose **New** to create a new log group, or **Existing** to use an existing one\. If you choose **New**, CloudTrail specifies a name for the new log group for you, or you can type a name\.

   1. If you choose **Existing**, choose a log group from the drop\-down list\.

   1. Choose **New** to create a new IAM role for permissions to send logs to CloudWatch Logs\. Choose **Existing** to choose an existing IAM role from the drop\-down list\. The policy statement for the new or existing role is displayed when you expand **Policy document**\. For more information about this role, see [Role policy document for CloudTrail to use CloudWatch Logs for monitoring](cloudtrail-required-policy-for-cloudwatch-logs.md)\.
**Note**  
When you configure a trail, you can choose an S3 bucket and SNS topic that belong to another account\. However, if you want CloudTrail to deliver events to a CloudWatch Logs log group, you must choose a log group that exists in your current account\.

1. For **Tags**, add one or more custom tags \(key\-value pairs\) to your trail\. Tags can help you identify both your CloudTrail trails and the Amazon S3 buckets that contain CloudTrail log files\. You can then use resource groups for your CloudTrail resources\. For more information, see [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html) and [Why use tags for trails?](cloudtrail-concepts.md#cloudtrail-concepts-tags)\.

1. On the **Choose log events** page, choose the event types that you want to log\. For **Management events**, do the following\.

   1. For **API activity**, choose if you want your trail to log **Read** events, **Write** events, or both\. For more information, see [Management events](logging-management-events-with-cloudtrail.md#logging-management-events)\.

   1. Choose **Exclude AWS KMS events** to filter AWS Key Management Service \(AWS KMS\) events out of your trail\. The default setting is to include all AWS KMS events\.

      The option to log or exclude AWS KMS events is available only if you log management events on your trail\. If you choose not to log management events, AWS KMS events are not logged, and you cannot change AWS KMS event logging settings\.

      AWS KMS actions such as `Encrypt`, `Decrypt`, and `GenerateDataKey` typically generate a large volume \(more than 99%\) of events\. These actions are now logged as **Read** events\. Low\-volume, relevant AWS KMS actions such as `Disable`, `Delete`, and `ScheduleKey` \(which typically account for less than 0\.5% of AWS KMS event volume\) are logged as **Write** events\.

      To exclude high\-volume events like `Encrypt`, `Decrypt`, and `GenerateDataKey`, but still log relevant events such as `Disable`, `Delete` and `ScheduleKey`, choose to log **Write** management events, and clear the check box for **Exclude AWS KMS events**\.

   1. Choose **Exclude Amazon RDS Data API events** to filter Amazon Relational Database Service Data API events out of your trail\. The default setting is to include all Amazon RDS Data API events\. For more information about Amazon RDS Data API events, see [Logging Data API calls with AWS CloudTrail](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/logging-using-cloudtrail-data-api.html) in the *Amazon RDS User Guide for Aurora*\.

1. For **Data events**, you can specify logging data events for Amazon S3 buckets, AWS Lambda functions, Amazon DynamoDB tables, or a combination of these resource types\. By default, trails don't log data events\. Additional charges apply for logging data events\. For more information, see [Data events](logging-data-events-with-cloudtrail.md#logging-data-events)\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\. More data event types are available if you use advanced event selectors; for more information, see [Creating a trail in the console \(advanced event selectors\)](cloudtrail-create-a-trail-using-the-console-first-time.md#creating-a-trail-in-the-console-adv) in this topic\.

   For Amazon S3 buckets:

   1. For **Data event source**, choose **S3**\.

   1. You can choose to log **All current and future S3 buckets**, or you can specify individual buckets or functions\. By default, data events are logged for all current and future S3 buckets\.
**Note**  
Keeping the default **All current and future S3 buckets** option enables data event logging for all buckets currently in your AWS account and any buckets you create after you finish creating the trail\. It also enables logging of data event activity performed by any IAM identity in your AWS account, even if that activity is performed on a bucket that belongs to another AWS account\.  
If you are creating a trail for a single Region \(done by using the AWS CLI\), choosing **All current and future S3 buckets** enables data event logging for all buckets in the same Region as your trail and any buckets you create later in that Region\. It will not log data events for Amazon S3 buckets in other Regions in your AWS account\.

   1. If you leave the default, **All current and future S3 buckets**, choose to log **Read** events, **Write** events, or both\.

   1. To select individual buckets, empty the **Read** and **Write** check boxes for **All current and future S3 buckets**\. In **Individual bucket selection**, browse for a bucket on which to log data events\. Find specific buckets by typing a bucket prefix for the bucket you want\. You can select multiple buckets in this window\. Choose **Add bucket** to log data events for more buckets\. Choose to log **Read** events, such as `GetObject`, **Write** events, such as `PutObject`, or both\.

      This setting takes precedence over individual settings you configure for individual buckets\. For example, if you specify logging **Read** events for all S3 buckets, and then choose to add a specific bucket for data event logging, **Read** is already selected for the bucket you added\. You cannot clear the selection\. You can only configure the option for **Write**\.

      To remove a bucket from logging, choose **X**\.

1. To add another data type on which to log data events, choose **Add data event type**\.

1. For Lambda functions:

   1. For **Data event source**, choose **Lambda**\.

   1. In **Lambda function**, choose **All regions** to log all Lambda functions, or **Input function as ARN** to log data events on a specific function\. 

      To log data events for all Lambda functions in your AWS account, select **Log all current and future functions**\. This setting takes precedence over individual settings you configure for individual functions\. All functions are logged, even if all functions are not displayed\.
**Note**  
If you are creating a trail for all Regions, this selection enables data event logging for all functions currently in your AWS account, and any Lambda functions you might create in any Region after you finish creating the trail\. If you are creating a trail for a single Region \(done by using the AWS CLI\), this selection enables data event logging for all functions currently in that Region in your AWS account, and any Lambda functions you might create in that Region after you finish creating the trail\. It does not enable data event logging for Lambda functions created in other Regions\.  
Logging data events for all functions also enables logging of data event activity performed by any IAM identity in your AWS account, even if that activity is performed on a function that belongs to another AWS account\.

   1. If you choose **Input function as ARN**, enter the ARN of a Lambda function\.
**Note**  
If you have more than 15,000 Lambda functions in your account, you cannot view or select all functions in the CloudTrail console when creating a trail\. You can still select the option to log all functions, even if they are not displayed\. If you want to log data events for specific functions, you can manually add a function if you know its ARN\. You can also finish creating the trail in the console, and then use the AWS CLI and the put\-event\-selectors command to configure data event logging for specific Lambda functions\. For more information, see [Managing trails with the AWS CLI](cloudtrail-additional-cli-commands.md)\.

1. For DynamoDB tables:

   1. For **Data event source**, choose **DynamoDB**\.

   1. In **DynamoDB table selection**, choose **Browse** to select a table, or paste in the ARN of a DynamoDB table to which you have access\. A DynamoDB table ARN uses the following format:

      ```
      arn:partition:dynamodb:region:account_ID:table/table_name
      ```

      To add another table, choose **Add row**, and browse for a table or paste in the ARN of a table to which you have access\.

1. Choose **Insights events** if you want your trail to log CloudTrail Insights events\.

   In **Event type**, select **Insights events**\. In **Insights events**, choose **API call rate**, **API error rate**, or both\. You must be logging **Write** management events to log Insights events for **API call rate**\. You must be logging **Read** or **Write** management events to log Insights events for **API error rate**\.

   CloudTrail Insights analyzes management events for unusual activity, and logs events when anomalies are detected\. By default, trails don't log Insights events\. For more information about Insights events, see [Logging Insights events for trails](logging-insights-events-with-cloudtrail.md)\. Additional charges apply for logging Insights events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

   Insights events are delivered to a different folder named `/CloudTrail-Insight`of the same S3 bucket that is specified in the **Storage location** area of the trail details page\. CloudTrail creates the new prefix for you\. For example, if your current destination S3 bucket is named `S3bucketName/AWSLogs/CloudTrail/`, the S3 bucket name with a new prefix is named `S3bucketName/AWSLogs/CloudTrail-Insight/`\.

1. When you are finished choosing event types to log, choose **Next**\.

1. On the **Review and create** page, review your choices\. Choose **Edit** in a section to change the trail settings shown in that section\. When you are ready to create the trail, choose **Create trail**\.

1. The new trail appears on the **Trails** page\. The **Trails** page shows the trails in your account from all Regions\. In about 5 minutes, CloudTrail publishes log files that show the AWS API calls made in your account\. You can see the log files in the S3 bucket that you specified\. It can take up to 36 hours for CloudTrail to deliver the first Insights event, if you have enabled Insights event logging, and unusual activity is detected\.

CloudTrail starts publishing log files that show the AWS calls made in your accounts in this region\. CloudTrail typically delivers logs within an average of about 5 minutes of an API call\. This time is not guaranteed\. Review the [AWS CloudTrail Service Level Agreement](http://aws.amazon.com/cloudtrail/sla) for more information\.

## Using the CLI to turn on CloudTrail in additional AWS accounts<a name="turn-on-cloudtrail-in-additional-accounts-cli"></a>

You can use the AWS command line tools to turn on CloudTrail in additional accounts and aggregate their log files to one Amazon S3 bucket\. For more information about these tools, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. 

Turn on CloudTrail in your additional accounts by using the `create-trail` command, specifying the following:
+ `--name` specifies the name of the trail\. 
+ `--s3-bucket-name` specifies the existing Amazon S3 bucket you created when you turned on CloudTrail in your first account \(111111111111 in this example\)\. 
+ `--s3-prefix` specifies a prefix for the log file delivery path \(optional\)\.
+ `--is-multi-region-trail` specifies that this trail will log events in all AWS Regions\. 

In contrast to trails that you create using the console, you must give every trail you create with the AWS CLI a name\. You can create one trail for each region in which an account is running AWS resources\. 

The following example command shows how to create a trail for your additional accounts by using the AWS CLI\. To have log files for these account delivered to the bucket you created in your first account \(111111111111 in this example\), specify the bucket name in the `--s3-bucket-name` option\. Amazon S3 bucket names are globally unique\. 

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail
```

When you run the command, you will see output similar to the following:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "AWSCloudTrailExample", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:222222222222:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": false,
    "S3BucketName": "MyBucketBelongingToAccount111111111111"
}
```

For more information about using CloudTrail from the AWS command line tools, see the [CloudTrail command line reference](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/index.html)\. 