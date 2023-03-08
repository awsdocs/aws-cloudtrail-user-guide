# Updating a trail<a name="cloudtrail-update-a-trail-console"></a>

To change trail settings, use the following procedure\.

To update a single\-region trail to log events in all regions, or update an all\-region trail to log events in only a single region, you must use the AWS CLI\. For more information about how to update a single\-region trail to log events in all regions, see [Converting a trail that applies to one Region to apply to all Regions](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert)\. For more information about how to update an all\-region trail to log events in a single region, see [Converting a multi\-region trail to a single\-region trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce)\.

**To update a trail with the AWS Management Console**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. In the navigation pane, choose **Trails**, and then choose a trail name\.

1. In **General details**, choose **Edit** to change the following settings\. You cannot change the name of a trail\.
   + **Apply trail to my organization** \- Change whether this trail is an AWS Organizations organization trail\.
   + **Trail log location** \- Change the name of the S3 bucket or prefix in which you are storing logs for this trail\.
   + **Log file SSE\-KMS encryption** \- Choose to enable or disable encrypting log files with SSE\-KMS instead of SSE\-S3\.
   + **Log file validation** \- Choose to enable or disable validation of the integrity of log files\.
   + **SNS notification delivery** \- Choose to enable or disable Amazon Simple Notification Service \(Amazon SNS\) notifications that log files have been delivered to the bucket specified for the trail\.

   1. To change the trail to an AWS Organizations organization trail, you can choose to enable the trail for all accounts in your organization\. For more information, see [Creating a trail for an organization](creating-trail-organization.md)\.

   1. To change the specified bucket in **Storage location**, choose **Create new S3 bucket** to create a bucket\. When you create a bucket, CloudTrail creates and applies the required bucket policies\.
**Note**  
If you chose **Use existing S3 bucket**, specify a bucket in **Trail log bucket name**, or choose **Browse** to choose a bucket\. The bucket policy must grant CloudTrail permission to write to it\. For information about manually editing the bucket policy, see [Amazon S3 bucket policy for CloudTrail](create-s3-bucket-policy-for-cloudtrail.md)\.

      To make it easier to find your logs, create a new folder \(also known as a *prefix*\) in an existing bucket to store your CloudTrail logs\. Enter the prefix in **Prefix**\.

   1. For **Log file SSE\-KMS encryption**, choose **Enabled** if you want to encrypt your log files using SSE\-KMS encryption instead of SSE\-S3 encryption\. The default is **Enabled**\. If you don't enable SSE\-KMS encryption, your logs are encrypted using SSE\-S3 encryption\. For more information about SSE\-KMS encryption, see [Using server\-side encryption with AWS Key Management Service \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)\. For more information about SSE\-S3 encryption, see [Using Server\-Side Encryption with Amazon S3\-Managed Encryption Keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html)\.

      If you enable SSE\-KMS encryption, choose a **New** or **Existing** AWS KMS key\. In **AWS KMS Alias**, specify an alias, in the format `alias/`*MyAliasName*\. For more information, see [Updating a resource to use your KMS key](create-kms-key-policy-for-cloudtrail-update-trail.md)\. CloudTrail also supports AWS KMS multi\-Region keys\. For more information about multi\-Region keys, see [Using multi\-Region keys](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html) in the *AWS Key Management Service Developer Guide*\.
**Note**  
You can also type the ARN of a key from another account\. For more information, see [Updating a resource to use your KMS key](create-kms-key-policy-for-cloudtrail-update-trail.md)\. The key policy must allow CloudTrail to use the key to encrypt your log files, and allow the users you specify to read log files in unencrypted form\. For information about manually editing the key policy, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\.

   1. For **Log file validation**, choose **Enabled** to have log digests delivered to your S3 bucket\. You can use the digest files to verify that your log files did not change after CloudTrail delivered them\. For more information, see [Validating CloudTrail log file integrity](cloudtrail-log-file-validation-intro.md)\.

   1. For **SNS notification delivery**, choose **Enabled** to be notified each time a log is delivered to your bucket\. CloudTrail stores multiple events in a log file\. SNS notifications are sent for every log file, not for every event\. For more information, see [Configuring Amazon SNS notifications for CloudTrail](configure-sns-notifications-for-cloudtrail.md)\.

      If you enable SNS notifications, for **Create a new SNS topic**, choose **New** to create a topic, or choose **Existing** to use an existing topic\. If you are creating a trail that applies to all Regions, SNS notifications for log file deliveries from all Regions are sent to the single SNS topic that you create\.

      If you choose **New**, CloudTrail specifies a name for the new topic for you, or you can type a name\. If you choose **Existing**, choose an SNS topic from the drop\-down list\. You can also enter the ARN of a topic from another Region or from an account with appropriate permissions\. For more information, see [Amazon SNS topic policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)\.

      If you create a topic, you must subscribe to the topic to be notified of log file delivery\. You can subscribe from the Amazon SNS console\. Due to the frequency of notifications, we recommend that you configure the subscription to use an Amazon SQS queue to handle notifications programmatically\. For more information, see the [Amazon Simple Notification Service Getting Started Guide](https://docs.aws.amazon.com/sns/latest/gsg/)\.

1. In **CloudWatch Logs**, choose **Edit** to change settings for sending CloudTrail log files to CloudWatch Logs\. Choose **Enabled** in **CloudWatch Logs** to enable sending log files\. For more information, see [Sending events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\.

   1. If you enable integration with CloudWatch Logs, choose **New** to create a new log group, or **Existing** to use an existing one\. If you choose **New**, CloudTrail specifies a name for the new log group for you, or you can type a name\.

   1. If you choose **Existing**, choose a log group from the drop\-down list\.

   1. Choose **New** to create a new IAM role for permissions to send logs to CloudWatch Logs\. Choose **Existing** to choose an existing IAM role from the drop\-down list\. The policy statement for the new or existing role is displayed when you expand **Policy document**\. For more information about this role, see [Role policy document for CloudTrail to use CloudWatch Logs for monitoring](cloudtrail-required-policy-for-cloudwatch-logs.md)\.
**Note**  
When you configure a trail, you can choose an S3 bucket and SNS topic that belong to another account\. However, if you want CloudTrail to deliver events to a CloudWatch Logs log group, you must choose a log group that exists in your current account\.
 If you have an existing organization trail with a CloudWatch Logs log group owned by the management account, the delegated administrator account can view the CloudWatch Logs log group in the console, but the delegated administrator account cannot update the CloudWatch Logs log group because it is owned by the management account\. The delegated administrator account cannot currently configure a CloudWatch Logs log group using the console, because the console operation is not supported\. The delegated administrator account must use the AWS CLI or CloudTrail API to update an organization trail with a CloudWatch Logs log group\. 

1. In **Tags**, choose **Edit** to change, add, or delete tags on the trail\. Add one or more custom tags \(key\-value pairs\) to your trail\. Tags can help you identify both your CloudTrail trails and the Amazon S3 buckets that contain CloudTrail log files\. You can then use resource groups for your CloudTrail resources\. For more information, see [AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html) and [Why use tags for trails?](cloudtrail-concepts.md#cloudtrail-concepts-tags)\.

1. In **Management events**, choose **Edit** to change management event logging settings\.

   1. For **API activity**, choose if you want your trail to log **Read** events, **Write** events, or both\. For more information, see [Management events](logging-management-events-with-cloudtrail.md#logging-management-events)\.

   1. Choose **Exclude AWS KMS events** to filter AWS Key Management Service \(AWS KMS\) events out of your trail\. The default setting is to include all AWS KMS events\.

      The option to log or exclude AWS KMS events is available only if you log management events on your trail\. If you choose not to log management events, AWS KMS events are not logged, and you cannot change AWS KMS event logging settings\.

      AWS KMS actions such as `Encrypt`, `Decrypt`, and `GenerateDataKey` typically generate a large volume \(more than 99%\) of events\. These actions are now logged as **Read** events\. Low\-volume, relevant AWS KMS actions such as `Disable`, `Delete`, and `ScheduleKey` \(which typically account for less than 0\.5% of AWS KMS event volume\) are logged as **Write** events\.

      To exclude high\-volume events like `Encrypt`, `Decrypt`, and `GenerateDataKey`, but still log relevant events such as `Disable`, `Delete` and `ScheduleKey`, choose to log **Write** management events, and clear the check box for **Exclude AWS KMS events**\.

   1. Choose **Exclude Amazon RDS Data API events** to filter Amazon Relational Database Service Data API events out of your trail\. The default setting is to include all Amazon RDS Data API events\. For more information about Amazon RDS Data API events, see [Logging Data API calls with AWS CloudTrail](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/logging-using-cloudtrail-data-api.html) in the *Amazon RDS User Guide for Aurora*\.

1. 
**Important**  
Steps 7\-9 are for configuring data events on your trail if you are using basic event selectors\. If you are using advanced event selectors, see [Updating data event settings with advanced event selectors](#cloudtrail-update-adv-event-selectors-console), then return to this procedure for Step 10 and forward\.

   In **Data events**, choose **Edit** to change data event logging settings\. With basic event selectors, you can specify logging data events for Amazon S3 buckets, AWS Lambda functions, DynamoDBtables, or a combination of those resources\. Additional data event types are supported with [advanced event selectors](#cloudtrail-update-adv-event-selectors-console)\. By default, trails don't log data events\. Additional charges apply for logging data events\. For more information, see [Data events](logging-data-events-with-cloudtrail.md#logging-data-events)\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

   For Amazon S3 buckets:

   1. For **Data event source**, choose **S3**\.

   1. You can choose to log **All current and future S3 buckets**, or you can specify individual buckets or functions\. By default, data events are logged for all current and future S3 buckets\.
**Note**  
Keeping the default **All current and future S3 buckets** option enables data event logging for all buckets currently in your AWS account and any buckets you create after you finish creating the trail\. It also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a bucket that belongs to another AWS account\.  
If the trail applies only to one Region, choosing **All current and future S3 buckets** enables data event logging for all buckets in the same Region as your trail and any buckets you create later in that Region\. It will not log data events for Amazon S3 buckets in other Regions in your AWS account\.

   1. If you leave the default, **All current and future S3 buckets**, choose to log **Read** events, **Write** events, or both\.

   1. To select individual buckets, empty the **Read** and **Write** check boxes for **All current and future S3 buckets**\. In **Individual bucket selection**, browse for a bucket on which to log data events\. To find specific buckets, type a bucket prefix for the bucket you want\. You can select multiple buckets in this window\. Choose **Add bucket** to log data events for more buckets\. Choose to log **Read** events, such as `GetObject`, **Write** events, such as `PutObject`, or both\.

      This setting takes precedence over individual settings you configure for individual buckets\. For example, if you specify logging **Read** events for all S3 buckets, and then choose to add a specific bucket for data event logging, **Read** is already selected for the bucket you added\. You cannot clear the selection\. You can only configure the option for **Write**\.

      To remove a bucket from logging, choose **X**\.

1. To add another data type on which to log data events, choose **Add data event type**\.

1. For Lambda functions:

   1. For **Data event source**, choose **Lambda**\.

   1. In **Lambda function**, choose **All regions** to log all Lambda functions, or **Input function as ARN** to log data events on a specific function\. 

      To log data events for all Lambda functions in your AWS account, select **Log all current and future functions**\. This setting takes precedence over individual settings you configure for individual functions\. All functions are logged, even if all functions are not displayed\.
**Note**  
If you are creating a trail for all Regions, this selection enables data event logging for all functions currently in your AWS account, and any Lambda functions you might create in any Region after you finish creating the trail\. If you are creating a trail for a single Region \(done by using the AWS CLI\), this selection enables data event logging for all functions currently in that Region in your AWS account, and any Lambda functions you might create in that Region after you finish creating the trail\. It does not enable data event logging for Lambda functions created in other Regions\.  
Logging data events for all functions also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a function that belongs to another AWS account\.

   1. If you choose **Input function as ARN**, enter the ARN of a Lambda function\.
**Note**  
If you have more than 15,000 Lambda functions in your account, you cannot view or select all functions in the CloudTrail console when creating a trail\. You can still select the option to log all functions, even if they are not displayed\. If you want to log data events for specific functions, you can manually add a function if you know its ARN\. You can also finish creating the trail in the console, and then use the AWS CLI and the put\-event\-selectors command to configure data event logging for specific Lambda functions\. For more information, see [Managing trails with the AWS CLI](cloudtrail-additional-cli-commands.md)\.

1. To add another data type on which to log data events, choose **Add data event type**\.

1. For DynamoDB tables:

   1. For **Data event source**, choose **DynamoDB**\.

   1. In **DynamoDB table selection**, choose **Browse** to select a table, or paste in the ARN of a DynamoDB table to which you have access\. A DynamoDB table ARN is in the following format:

      ```
      arn:partition:dynamodb:region:account_ID:table/table_name
      ```

      To add another table, choose **Add row**, and browse for a table or paste in the ARN of a table to which you have access\.

1. In **Insights events** choose **Edit** if you want your trail to log CloudTrail Insights events\.

   In **Event type**, select **Insights events**\. 

    In **Insights events**, choose **API call rate**, **API error rate**, or both\. You must be logging **Write** management events to log Insights events for **API call rate**\. You must be logging **Read** or **Write** management events to log Insights events for **API error rate**\.

   CloudTrail Insights analyzes management events for unusual activity, and logs events when anomalies are detected\. By default, trails don't log Insights events\. For more information about Insights events, see [Logging Insights events for trails](logging-insights-events-with-cloudtrail.md)\. Additional charges apply for logging Insights events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

   Insights events are delivered to a different folder named `/CloudTrail-Insight`of the same S3 bucket that is specified in the **Storage location** area of the trail details page\. CloudTrail creates the new prefix for you\. For example, if your current destination S3 bucket is named `S3bucketName/AWSLogs/CloudTrail/`, the S3 bucket name with a new prefix is named `S3bucketName/AWSLogs/CloudTrail-Insight/`\.

1. When you are finished changing settings on your trail, choose **Update trail**\.

## Updating data event settings with advanced event selectors<a name="cloudtrail-update-adv-event-selectors-console"></a>

1. On the trail's details page, in **Data events**, choose **Edit**\.

1. If you are not already logging data events, choose **Data events**\.

1. For **Data event type**, choose the resource type on which you want to log data events\.
**Note**  
To log data events for AWS Glue tables created by Lake Formation, choose **Lake Formation**\.

1. Choose a log selector template\. CloudTrail includes predefined templates that log all data events for the resource type\. To build a custom log selector template, choose **Custom**\.
**Note**  
Choosing a predefined template for S3 buckets enables data event logging for all buckets currently in your AWS account and any buckets you create after you finish creating the trail\. It also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a bucket that belongs to another AWS account\.  
If the trail applies only to one Region, choosing a predefined template that logs all S3 buckets enables data event logging for all buckets in the same Region as your trail and any buckets you create later in that Region\. It will not log data events for Amazon S3 buckets in other Regions in your AWS account\.  
If you are creating a trail for all Regions, choosing a predefined template for Lambda functions enables data event logging for all functions currently in your AWS account, and any Lambda functions you might create in any Region after you finish creating the trail\. If you are creating a trail for a single Region \(done by using the AWS CLI\), this selection enables data event logging for all functions currently in that Region in your AWS account, and any Lambda functions you might create in that Region after you finish creating the trail\. It does not enable data event logging for Lambda functions created in other Regions\.  
Logging data events for all functions also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a function that belongs to another AWS account\.

1. If you want to apply a predefined log selector template, and you do not want to add another data event resource type, choose **Save changes**, and skip to the last step of this procedure\. To apply a custom log selector template, go on to the next step\.

1. To create a custom log selector template, in the **Log selector template** drop\-down list, choose **Custom**\.

1. Optionally, enter a name for your custom log selector template\.

1. In **Advanced event selectors**, build an expression for the specific resources on which you want to collect data events\.

   1. Choose from the following fields\. For fields that accept an array \(more than one value\), CloudTrail adds an OR between values\.
      + **`readOnly`** \- `readOnly` can be set to **Equals** a value of `true` or `false`\. To log both `read` and `write` events, don't add a `readOnly` selector\.
      + **`eventName`** \- `eventName` can use any operator\. You can use it to include or exclude any data event logged to CloudTrail, such as `PutBucket` or `GetSnapshotBlock`\. You can have multiple values for this field, separated by commas\.
      + **`resources.type`** \- In the AWS Management Console, this field doesn't occur, because it's already populated by your choice of data event type from the **Data event type** drop\-down list\. In the AWS CLI and SDKs, `resources.type` can only use the **Equals** operator, and the value can be one of the following: 
        + `AWS::DynamoDB::Table`
        + `AWS::Lambda::Function`
        + `AWS::S3::Object`
        + `AWS::CloudTrail::Channel`
        + `AWS::Cognito::IdentityPool`
        + `AWS::DynamoDB::Stream`
        + `AWS::EC2::InstanceConnectEndpoint`
        + `AWS::EC2::Snapshot`
        + `AWS::Glue::Table`
        + `AWS::GuardDuty::Detector`
        + `AWS::FinSpace::Environment`
        + `AWS::KendraRanking::ExecutionPlan`
        + `AWS::ManagedBlockchain::Node`
        + `AWS::SageMaker::ExperimentTrialComponent`
        + `AWS::SageMaker::FeatureGroup`
        + `AWS::S3::AccessPoint`
        + `AWS::S3ObjectLambda::AccessPoint`
        + `AWS::S3Outposts::Object`
      + **`resources.ARN`** \- You can use any operator with `resources.ARN`, but if you use **Equals** or **NotEquals**, the value must exactly match the ARN of a valid resource of the type you've specified in the template as the value of `resources.type`\. 

        The following table shows the valid ARN format for each `resources.Type`\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-update-a-trail-console.html)

        1 To log all data events for all objects in a specific S3 bucket, use the `StartsWith` operator, and include only the bucket ARN as the matching value\. The trailing slash is intentional; do not exclude it\.

        2 To log events on all objects in an S3 access point, we recommend that you use only the access point ARN, don’t include the object path, and use the `StartsWith` or `NotStartsWith` operators\.

      For more information about the ARN formats of data event resources, see [Actions, resources, and condition keys](https://docs.aws.amazon.com/service-authorization/latest/reference/reference_policies_actions-resources-contextkeys.html) in the *AWS Identity and Access Management User Guide*\.

   1. For each field, choose **\+ Conditions** to add as many conditions as you need, up to a maximum of 500 specified values for all conditions\. For example, to exclude data events for two S3 buckets from data events that are logged on your trail, you can set the field to **resources\.ARN**, set the operator for **NotStartsWith**, and then either paste in an S3 bucket ARN, or browse for the S3 buckets for which you do not want to log events\.

      To add the second S3 bucket, choose **\+ Conditions**, and then repeat the preceding instruction, pasting in the ARN for or browsing for a different bucket\.
**Note**  
You can have a maximum of 500 values for all selectors on a trail\. This includes arrays of multiple values for a selector such as `eventName`\. If you have single values for all selectors, you can have a maximum of 500 conditions added to a selector\.  
If you have more than 15,000 Lambda functions in your account, you cannot view or select all functions in the CloudTrail console when creating a trail\. You can still log all functions with a predefined selector template, even if they are not displayed\. If you want to log data events for specific functions, you can manually add a function if you know its ARN\. You can also finish creating the trail in the console, and then use the AWS CLI and the put\-event\-selectors command to configure data event logging for specific Lambda functions\. For more information, see [Managing trails with the AWS CLI](cloudtrail-additional-cli-commands.md)\.

   1. Choose **\+ Field** to add additional fields as required\. To avoid errors, do not set conflicting or duplicate values for fields\. For example, do not specify an ARN in one selector to be equal to a value, then specify that the ARN not equal the same value in another selector\.

   1. Save changes to your custom selector template by choosing **Next**\. Do not choose another log selector template, or leave this page, or your custom selectors will be lost\.

1. To add another data type on which to log data events, choose **Add data event type**\. Repeat steps 3 through this step to configure advanced event selectors for the data event type\.

1. After you choose **Next**, in **Step 2: Choose log events**, review the log selector template options you've chosen\. Choose **Edit** to go back and make changes\.

1. To configure Insights events and other settings for your trail, go back to the preceding procedure in this topic, [Updating a trail](#cloudtrail-update-a-trail-console)\.