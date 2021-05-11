# Creating, updating, and managing trails with the AWS Command Line Interface<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli"></a>

You can use the AWS CLI to create, update, and manage your trails\. When using the AWS CLI, remember that your commands run in the AWS Region configured for your profile\. If you want to run the commands in a different Region, either change the default Region for your profile, or use the \-\-region parameter with the command\.

**Note**  
You need the AWS command line tools to run the AWS Command Line Interface \(AWS CLI\) commands in this topic\. Make sure you have a recent version of the AWS CLI installed\. For more information, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For help with CloudTrail commands at the AWS CLI command line, type `aws cloudtrail help`\.

## Commonly used commands for trail creation, management, and status<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-options"></a>

Some of the more commonly used commands for creating and updating trails in CloudTrail include: 
+ [create\-trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail.md) to create a trail\.
+ [update\-trail](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-update-trail.md) to change the configuration of an existing trail\.
+ [add\-tags](cloudtrail-additional-cli-commands.md#cloudtrail-additional-cli-commands-add-tag) to add one or more tags \(key\-value pairs\) to an existing trail\.
+ [remove\-tags](cloudtrail-additional-cli-commands.md#cloudtrail-additional-cli-commands-remove-tag) to remove one or more tags from a trail\.
+ [list\-tags](cloudtrail-additional-cli-commands.md#cloudtrail-additional-cli-commands-list-tags) to return a list of tags associated with a trail\.
+ [put\-event\-selectors](cloudtrail-additional-cli-commands.md#configuring-event-selector-examples) to add or modify event selectors for a trail\.
+ [put\-insight\-selectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutInsightSelectors.html) to add or modify Insights event selectors for an existing trail, and enable or disable Insights events\.
+ [start\-logging](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-create-trail.md#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-single-start-logging) to begin logging events with your trail\.
+ [stop\-logging](cloudtrail-additional-cli-commands.md#cloudtrail-start-stop-logging-cli-commands) to pause logging events with your trail\.
+ [delete\-trail](cloudtrail-additional-cli-commands.md#cloudtrail-delete-trail-cli) to delete a trail\. This command does not delete the Amazon S3 bucket that contains the log files for that trail, if any\.
+ [describe\-trails](cloudtrail-additional-cli-commands.md#cloudtrail-additional-cli-commands-retrieve) to return information about trails in an AWS Region\.
+ [get\-trail](cloudtrail-additional-cli-commands.md#cloudtrail-additional-cli-commands-retrieve) to return settings information for a trail\.
+  [get\-trail\-status](cloudtrail-additional-cli-commands.md#cloudtrail-additional-cli-commands-retrieve) to return information about the current status of a trail\.
+ [get\-event\-selectors](cloudtrail-additional-cli-commands.md#configuring-event-selector-examples) to return information about event selectors configured for a trail\.
+ [get\-insight\-selectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_GetInsightSelectors.html) to return information about Insights event selectors configured for a trail\.

### Supported commands for creating and updating trails: create\-trail and update\-trail<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-ctut"></a>

The `create-trail` and `update-trail` commands offer a variety of functionality for creating and managing trails, including:
+ Creating a trail that receives logs across Regions, or update a trail with the `--is-multi-region-trail` option\. In most circumstances, you should create trails that log events in all AWS Regions\.
+ Creating a trail that receives logs for all AWS accounts in an organization with the \-\-is\-organization\-trail option\.
+ Converting a multi\-region trail to single\-region trail with the `--no-is-multi-region-trail` option\.
+ Enabling or disabling log file encryption with the `--kms-key-id` option\. The option specifies an AWS KMS key that you already created and to which you have attached a policy that allows CloudTrail to encrypt your logs\. For more information, see [Enabling and disabling CloudTrail log file encryption with the AWS CLI](cloudtrail-log-file-encryption-cli.md)\.
+ Enabling or disabling log file validation with the `--enable-log-file-validation` and `--no-enable-log-file-validation` options\. For more information, see [Validating CloudTrail Log File Integrity](cloudtrail-log-file-validation-intro.md)\.
+ Specifying a CloudWatch Logs log group and role so that CloudTrail can deliver events to a CloudWatch Logs log group\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.

### Deprecated commands: create\-subscription and update\-subscription<a name="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-subs"></a>

**Important**  
The `create-subscription` and `update-subscription` commands were used to create and update trails, but are deprecated\. Do not use these commands\. They do not provide full functionality for creating and managing trails\.  
If you configured automation that uses one or both of these commands, we recommend that you update your code or scripts to use supported commands such as create\-trail\.