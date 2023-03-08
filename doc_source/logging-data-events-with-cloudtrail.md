# Logging data events<a name="logging-data-events-with-cloudtrail"></a>

By default, trails and event data stores do not log data events\. Additional charges apply for data events\. For more information, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

**Note**  
The events that are logged by your trails and event data stores are available in Amazon CloudWatch Events\. For example, if you configure a trail to log data events for S3 objects but not management events, your trail processes and logs only data events for the specified S3 objects\. The data events for these S3 objects are available in Amazon CloudWatch Events\. For more information, see [AWS API Call Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#api_event_type) in the *Amazon CloudWatch Events User Guide*\. 

**Contents**
+ [Data events](#logging-data-events)
  + [Examples: Logging data events for Amazon S3 objects](#logging-data-events-examples)
  + [Logging data events for S3 objects in other AWS accounts](#logging-data-events-for-s3-resources-in-other-accounts)
+ [Read\-only and write\-only events](#read-write-events-data)
+ [Logging data events with the AWS Command Line Interface](#creating-data-event-selectors-with-the-AWS-CLI)
  + [Examples: Logging data events for trails](#logging-data-events-CLI-trail-examples)
    + [Log events by using basic event selectors](#creating-data-event-selectors-basic)
    + [Log events by using advanced event selectors](#creating-data-event-selectors-advanced)
    + [Log all Amazon S3 events for a bucket by using advanced event selectors](#creating-data-adv-event-selectors-CLI-s3)
    + [Log Amazon S3 on AWS Outposts events by using advanced event selectors](#creating-data-event-selectors-CLI-outposts)
    + [Log all data events by using advanced event selectors](#creating-data-adv-event-selectors-CLI-all-data)
  + [Examples: Logging data events for event data stores](#logging-data-events-CLI-eds-examples)
    + [Include all Amazon S3 events for a bucket](#creating-data-adv-event-selectors-CLI-s3-eds)
    + [Include Amazon S3 on AWS Outposts events](#creating-data-event-selectors-CLI-outposts-eds)
+ [Logging data events for AWS Config compliance](#config-data-events-best-practices)
+ [Logging events with the AWS SDKs](#logging-data-events-with-the-AWS-SDKs)
+ [Sending events to Amazon CloudWatch Logs](#sending-data-events-to-cloudwatch-logs)

## Data events<a name="logging-data-events"></a>

Data events provide visibility into the resource operations performed on or within a resource\. These are also known as data plane operations\. Data events are often high\-volume activities\.

The following table shows the data event types available for trails and event data stores\. The **Data event type \(console\)** column shows the appropriate selection in the console\. The ** resources\.Type** column shows the `resources.Type` value that you would specify to include data events of that type in your trail or event data store\.

Trails can use basic or advanced event selectors\. The **Event selector type** column of the table identities for trails whether the data event type is selectable using basic or advanced event selectors\. The first three rows of the table show the data event types selectable with basic event selectors\. The remaining rows show the data event types that you can specify using advanced event selectors\. 

Unlike trails, event data stores only use advanced event selectors to specify data events\.


****  

| AWS service | Event selector type | Data event type \(console\) | resources\.Type | Description | 
| --- | --- | --- | --- | --- | 
| Amazon DynamoDB | basic | DynamoDB | AWS::DynamoDB::Table | Amazon DynamoDB object\-level API activity on tables \(for example, `PutItem`, `DeleteItem`, and `UpdateItem` API operations\)\. For more information about DynamoDB events, see [DynamoDB data plane events in CloudTrail](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html#ddb-data-plane-events-in-cloudtrail)\. | 
| AWS Lambda | basic | Lambda | AWS::Lambda::Function | AWS Lambda function execution activity \(the `Invoke` API\)\. | 
| Amazon S3 | basic | S3 | AWS::S3::Object | Amazon S3 object\-level API activity \(for example, `GetObject`, `DeleteObject`, and `PutObject` API operations\) on buckets and objects in buckets\. | 
| AWS CloudTrail | advanced | CloudTrail | AWS::CloudTrail::Channel | CloudTrail [https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html](https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html) activity on a [CloudTrail Lake channel](query-event-data-store-integration.md) that is used to log events from outside AWS\. | 
| Amazon Cognito | advanced | Cognito Identity Pools | AWS::Cognito::IdentityPool | Amazon Cognito API activity on Amazon Cognito [identity pools](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-info-in-cloudtrail.html#identity-pools-cloudtrail-events)\. | 
| Amazon DynamoDB | advanced | DynamoDB Streams | AWS::DynamoDB::Stream | [Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html#ddb-data-plane-events-in-cloudtrail) API activity on streams | 
| Amazon Elastic Block Store | advanced | Amazon EBS direct APIs | AWS::EC2::Snapshot | [Amazon Elastic Block Store \(EBS\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/logging-ebs-apis-using-cloudtrail.html) direct APIs, such as `PutSnapshotBlock`, `GetSnapshotBlock`, and `ListChangedBlocks` on Amazon EBS snapshots | 
| Amazon EC2 Instance Connect | advanced | Amazon EC2 Instance connect endpoint | AWS::EC2::InstanceConnectEndpoint | Amazon EC2 Instance Connect API activity on endpoints\. | 
| Amazon FinSpace | advanced | FinSpace | AWS::FinSpace::Environment | [Amazon FinSpace](https://docs.aws.amazon.com/finspace/latest/userguide/logging-cloudtrail-events.html#finspace-dataplane-events) API activity on environments | 
| AWS Glue | advanced | Lake Formation | AWS::Glue::Table | AWS Glue API activity on tables that were created by Lake Formation  AWS Glue data events for tables are currently supported only in the following regions:   US East \(N\. Virginia\)   US East \(Ohio\)   US West \(Oregon\)   Europe \(Ireland\)   Asia Pacific \(Tokyo\) Region    | 
| Amazon GuardDuty | advanced | GuardDuty detector | AWS::GuardDuty::Detector | Amazon GuardDuty API activity on detectors\. | 
| Amazon Kendra Intelligent Ranking | advanced | Kendra Ranking | AWS::KendraRanking::ExecutionPlan | Amazon Kendra Intelligent Ranking API activity on [rescore execution plans](https://docs.aws.amazon.com/kendra/latest/dg/cloudtrail-intelligent-ranking.html#cloud-trail-intelligent-ranking-log-entry)\. | 
| Amazon Managed Blockchain | advanced | Managed Blockchain | AWS::ManagedBlockchain::Node | [Amazon Managed Blockchain](https://docs.aws.amazon.com/managed-blockchain/latest/ethereum-dev/logging-using-cloudtrail.html#ethereum-jsonrpc-logging) JSON\-RPC calls on Ethereum nodes, such as `eth_getBalance` or `eth_getBlockByNumber`\. | 
| Amazon SageMaker | advanced | SageMaker feature store | AWS::SageMaker::FeatureGroup | Amazon SageMaker API activity on feature stores | 
| Amazon SageMaker | advanced | SageMaker metrics experiment trial component | AWS::SageMaker::ExperimentTrialComponent | Amazon SageMaker API activity on [experiment trial components](https://docs.aws.amazon.com/sagemaker/latest/dg/experiments-monitoring.html) | 
| Amazon S3 | advanced | S3 Access Point | AWS::S3::AccessPoint | Amazon S3 API activity on access points | 
| Amazon S3 | advanced | S3 Object Lambda | AWS::S3ObjectLambda::AccessPoint | Amazon S3 Object Lambda access points API activity, such as calls to `CompleteMultipartUpload` and `GetObject`\. | 
| Amazon S3 on Outposts | advanced | S3 Outposts | AWS::S3Outposts::Object |  Amazon S3 on Outposts object\-level API activity\. | 

Data events are not logged by default when you create a trail or event data store\. To record CloudTrail data events, you must explicitly add the supported resources or resource types for which you want to collect activity\. For more information, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Create an event data store](query-event-data-store.md)\.

On a single\-Region trail or event data store, you can log data events only for resources that you can access in that Region\. Though S3 buckets are global, AWS Lambda functions and DynamoDB tables are regional\.

Additional charges apply for logging data events\. For CloudTrail pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

Steps for logging data events depend on whether you have advanced event selectors enabled\. Use the procedure in this section that matches the kind of event selectors you have enabled\.

### Logging data events with basic event selectors in the AWS Management Console<a name="logging-data-events-with-the-cloudtrail-console"></a>

Use the following procedure to log data events using basic event selectors\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Open the **Trails** page of the CloudTrail console and choose the trail name\.
**Note**  
While you can edit an existing trail to log data events, as a best practice, consider creating a separate trail specifically for logging data events\.

1. For **Data events**, choose **Edit**\.

1. For Amazon S3 buckets:

   1. For **Data event source**, choose **S3**\.

   1. You can choose to log **All current and future S3 buckets**, or you can specify individual buckets or functions\. By default, data events are logged for all current and future S3 buckets\.
**Note**  
Keeping the default **All current and future S3 buckets** option enables data event logging for all buckets currently in your AWS account and any buckets you create after you finish creating the trail\. It also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a bucket that belongs to another AWS account\.  
If you are creating a trail for a single Region \(done by using the AWS CLI\), selecting the **Select all S3 buckets in your account** option enables data event logging for all buckets in the same Region as your trail and any buckets you create later in that Region\. It will not log data events for Amazon S3 buckets in other Regions in your AWS account\.

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

   1. In **DynamoDB table selection**, choose **Browse** to select a table, or paste in the ARN of a DynamoDB table to which you have access\. A DynamoDB table ARN uses the following format:

      ```
      arn:partition:dynamodb:region:account_ID:table/table_name
      ```

      To add another table, choose **Add row**, and browse for a table or paste in the ARN of a table to which you have access\.

1. Choose **Save changes**\.

### Logging data events with advanced event selectors in the AWS Management Console<a name="logging-data-events-with-the-cloudtrail-console-adv"></a>

In the AWS Management Console, if you have advanced event selectors enabled, you can choose from predefined templates that log all data events on a selected resource \(Amazon S3 buckets or access points, Lambda functions, S3 objects on AWS Outposts, Ethereum for Managed Blockchain nodes, or S3 Object Lambda access points\)\. After you choose a log selector template, you can customize the template to include only the data events you most want to see\. For more information and tips about using advanced event selectors, see [Log events by using advanced event selectors](#creating-data-event-selectors-advanced) in this topic\.

1. On the **Dashboard**, **Trails**, or **Event data stores** pages of the CloudTrail console, choose the trail or event data store you want to update\.
**Note**  
You can only enable data events on event data stores that contain CloudTrail events\. You cannot enable data events on CloudTrail event data stores for AWS Config configuration items or non\-AWS events\.

1. On the details page, in **Data events**, choose **Edit**\.

1. If you are not already logging data events, choose the **Data events** check box\.

1. For **Data event type**, choose the resource type on which you want to log data events\.

1. Choose a log selector template\. CloudTrail includes predefined templates that log all data events for the resource type\. To build a custom log selector template, choose **Custom**\.
**Note**  
Choosing a predefined template for S3 buckets enables data event logging for all buckets currently in your AWS account and any buckets you create after you finish creating the trail\. It also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a bucket that belongs to another AWS account\.  
If the trail applies only to one Region, choosing a predefined template that logs all S3 buckets enables data event logging for all buckets in the same Region as your trail and any buckets you create later in that Region\. It will not log data events for Amazon S3 buckets in other Regions in your AWS account\.  
If you are creating a trail for all Regions, choosing a predefined template for Lambda functions enables data event logging for all functions currently in your AWS account, and any Lambda functions you might create in any Region after you finish creating the trail\. If you are creating a trail for a single Region \(done by using the AWS CLI\), this selection enables data event logging for all functions currently in that Region in your AWS account, and any Lambda functions you might create in that Region after you finish creating the trail\. It does not enable data event logging for Lambda functions created in other Regions\.  
Logging data events for all functions also enables logging of data event activity performed by any user or role in your AWS account, even if that activity is performed on a function that belongs to another AWS account\.

1. If you want to apply a predefined log selector template, and you do not want to add another data event resource type, choose **Save changes**\. You do not need to follow the rest of this procedure\. To apply a custom log selector template, go on to the next step\.

1. To create a custom log selector template, in the **Log selector template** drop\-down list, choose **Custom**\.

1. \(Optional\) Enter a name for your custom log selector template\.

1. In **Advanced event selectors**, build an expression to collect data events on specific S3 buckets, AWS Lambda functions, `PutAuditEvents` calls on CloudTrail Lake channels, DynamoDB tables, Amazon S3 on Outposts, Amazon Managed Blockchain JSON\-RPC calls on Ethereum nodes, S3 Object Lambda access points, Amazon EBS direct APIs on EBS snapshots, S3 access points, DynamoDB streams, AWS Glue tables created by Lake Formation, Amazon FinSpace environments, Amazon SageMaker metrics experiment trial components, Amazon SageMaker feature stores, Amazon Kendra rescore execution plans, Amazon Cognito identity pools, Amazon EC2 instance connect endpoints, and Amazon GuardDuty detectors\.

   1. Choose from the following fields\. For fields that accept an array \(more than one value\), CloudTrail adds an OR between values\.
      + **`readOnly`** \- `readOnly` can be set to **Equals** a value of `true` or `false`\. Read\-only data events are events that do not change the state of a resource, such as `Get*` or `Describe*` events\. Write events add, change, or delete resources, attributes, or artifacts, such as `Put*`, `Delete*`, or `Write*` events\. To log both `read` and `write` events, don't add a `readOnly` selector\.
      + **`eventName`** \- `eventName` can use any operator\. You can use it to include or exclude any data event logged to CloudTrail, such as `PutBucket`, `GetItem`, or `GetSnapshotBlock`\. You can have multiple values for this field, separated by commas\.
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
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)

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

1. To add another data type on which to log data events, choose **Add data event type**\. Repeat steps 4 through this step to configure advanced event selectors for the data event type\.

1. After you choose **Next**, in **Step 2: Choose log events**, review the log selector template options you've chosen\. Choose **Edit** to go back and make changes\.

1. After you've reviewed and verified your choices, choose **Save changes** if this is an existing trail or event data store, or **Create trail** or **Create event data store** if you are creating a new trail or event data store\.

### Examples: Logging data events for Amazon S3 objects<a name="logging-data-events-examples"></a>

**Logging data events for all S3 objects in an S3 bucket**

The following example demonstrates how logging works when you configure logging of all data events for an S3 bucket named *bucket\-1*\. In this example, the CloudTrail user specified an empty prefix, and the option to log both **Read** and **Write** data events\.

1. A user uploads an object to `bucket-1`\. 

1. The `PutObject` API operation is an Amazon S3 object\-level API\. It is recorded as a data event in CloudTrail\. Because the CloudTrail user specified an S3 bucket with an empty prefix, events that occur on any object in that bucket are logged\. The trail processes and logs the event\.

1. Another user uploads an object to `bucket-2`\. 

1. The `PutObject` API operation occurred on an object in an S3 bucket that wasn't specified for the trail\. The trail doesn't log the event\. 

**Logging data events for specific S3 objects**

The following example demonstrates how logging works when you configure a trail to log events for specific S3 objects\. In this example, the CloudTrail user specified an S3 bucket named *bucket\-3*, with the prefix *my\-images*, and the option to log only **Write** data events\.

1. A user deletes an object that begins with the `my-images` prefix in the bucket, such as `arn:aws:s3:::bucket-3/my-images/example.jpg`\.

1. The `DeleteObject` API operation is an Amazon S3 object\-level API\. It is recorded as a **Write** data event in CloudTrail\. The event occurred on an object that matches the S3 bucket and prefix specified in the trail\. The trail processes and logs the event\.

1. Another user deletes an object with a different prefix in the S3 bucket, such as `arn:aws:s3:::bucket-3/my-videos/example.avi`\.

1. The event occurred on an object that doesn't match the prefix specified in your trail\. The trail doesn't log the event\.

1. A user calls the `GetObject` API operation for the object, `arn:aws:s3:::bucket-3/my-images/example.jpg`\.

1. The event occurred on a bucket and prefix that are specified in the trail, but `GetObject` is a read\-type Amazon S3 object\-level API\. It is recorded as a **Read** data event in CloudTrail, and the trail is not configured to log **Read** events\. The trail doesn't log the event\.

**Note**  
If you are logging data events for specific Amazon S3 buckets, we recommend you do not use an Amazon S3 bucket for which you are logging data events to receive log files that you have specified in the data events section\. Using the same Amazon S3 bucket causes your trail to log a data event each time log files are delivered to your Amazon S3 bucket\. Log files are aggregated events delivered at intervals, so this is not a 1:1 ratio of event to log file; the event is logged in the next log file\. For example, when the trail delivers logs, the `PutObject` event occurs on the S3 bucket\. If the S3 bucket is also specified in the data events section, the trail processes and logs the `PutObject` event as a data event\. That action is another `PutObject` event, and the trail processes and logs the event again\. For more information, see [How CloudTrail works](how-cloudtrail-works.md)\.  
To avoid logging data events for the Amazon S3 bucket where you receive log files if you configure a trail to log all Amazon S3 data events in your AWS account, consider configuring delivery of log files to an Amazon S3 bucket that belongs to another AWS account\. For more information, see [Receiving CloudTrail log files from multiple accountsRedacting bucket owner account IDs for data events called by other accounts](cloudtrail-receive-logs-from-multiple-accounts.md)\.

### Logging data events for S3 objects in other AWS accounts<a name="logging-data-events-for-s3-resources-in-other-accounts"></a>

When you configure your trail to log data events, you can also specify S3 objects that belong to other AWS accounts\. When an event occurs on a specified object, CloudTrail evaluates whether the event matches any trails in each account\. If the event matches the settings for a trail, the trail processes and logs the event for that account\. Generally, both API callers and resource owners can receive events\.

If you own an S3 object and you specify it in your trail, your trail logs events that occur on the object in your account\. Because you own the object, your trail also logs events when other accounts call the object\.

If you specify an S3 object in your trail, and another account owns the object, your trail only logs events that occur on that object in your account\. Your trail doesn't log events that occur in other accounts\.

**Example: Logging data events for an Amazon S3 object for two AWS accounts**

The following example shows how two AWS accounts configure CloudTrail to log events for the same S3 object\.

1. In your account, you want your trail to log data events for all objects in your S3 bucket named `owner-bucket`\. You configure the trail by specifying the S3 bucket with an empty object prefix\.

1. Bob has a separate account that has been granted access to the S3 bucket\. Bob also wants to log data events for all objects in the same S3 bucket\. For his trail, he configures his trail and specifies the same S3 bucket with an empty object prefix\.

1. Bob uploads an object to the S3 bucket with the `PutObject` API operation\.

1. This event occurred in his account and it matches the settings for his trail\. Bob's trail processes and logs the event\.

1. Because you own the S3 bucket and the event matches the settings for your trail, your trail also processes and logs the same event\. Because there are now two copies of the event \(one logged in Bob's trail, and one logged in yours\), CloudTrail charges for two copies of the data event\.

1. You upload an object to the S3 bucket\.

1. This event occurs in your account and it matches the settings for your trail\. Your trail processes and logs the event\.

1. Because the event didn't occur in Bob's account, and he doesn't own the S3 bucket, Bob's trail doesn't log the event\. CloudTrail charges for only one copy of this data event\.

**Example: Logging data events for all buckets, including an S3 bucket used by two AWS accounts**

The following example shows the logging behavior when **Select all S3 buckets in your account** is enabled for trails that collect data events in an AWS account\.

1. In your account, you want your trail to log data events for all S3 buckets\. You configure the trail by choosing **Read** events, **Write** events, or both for **All current and future S3 buckets** in **Data events**\.

1. Bob has a separate account that has been granted access to an S3 bucket in your account\. He wants to log data events for the bucket to which he has access\. He configures his trail to get data events for all S3 buckets\.

1. Bob uploads an object to the S3 bucket with the `PutObject` API operation\.

1. This event occurred in his account and it matches the settings for his trail\. Bob's trail processes and logs the event\.

1. Because you own the S3 bucket and the event matches the settings for your trail, your trail also processes and logs the event\. Because there are now two copies of the event \(one logged in Bob's trail, and one logged in yours\), CloudTrail charges each account for a copy of the data event\.

1. You upload an object to the S3 bucket\.

1. This event occurs in your account and it matches the settings for your trail\. Your trail processes and logs the event\.

1. Because the event didn't occur in Bob's account, and he doesn't own the S3 bucket, Bob's trail doesn't log the event\. CloudTrail charges for only one copy of this data event in your account\.

1. A third user, Mary, has access to the S3 bucket, and runs a `GetObject` operation on the bucket\. She has a trail configured to log data events on all S3 buckets in her account\. Because she is the API caller, CloudTrail logs a data event in her trail\. Though Bob has access to the bucket, he is not the resource owner, so no event is logged in his trail this time\. As the resource owner, you receive an event in your trail about the `GetObject` operation that Mary called\. CloudTrail charges your account and Mary's account for each copy of the data event: one in Mary's trail, and one in yours\.

## Read\-only and write\-only events<a name="read-write-events-data"></a>

When you configure your trail to log data and management events, you can specify whether you want read\-only events, write\-only events, or both\.
+ **Read**

  **Read** events include API operations that read your resources, but don't make changes\. For example, read\-only events include the Amazon EC2 `DescribeSecurityGroups` and `DescribeSubnets` API operations\. These operations return only information about your Amazon EC2 resources and don't change your configurations\. 
+ **Write**

  **Write** events include API operations that modify \(or might modify\) your resources\. For example, the Amazon EC2 `RunInstances` and `TerminateInstances` API operations modify your instances\.

**Example: Logging read and write events for separate trails**

The following example shows how you can configure trails to split log activity for an account into separate S3 buckets: one bucket receives read\-only events and a second bucket receives write\-only events\.

1. You create a trail and choose an S3 bucket named `read-only-bucket` to receive log files\. You then update the trail to specify that you want **Read** management events and data events\.

1. You create a second trail and choose an S3 bucket named `write-only-bucket` to receive log files\. You then update the trail to specify that you want **Write** management events and data events\.

1. The Amazon EC2 `DescribeInstances` and `TerminateInstances` API operations occur in your account\.

1. The `DescribeInstances` API operation is a read\-only event and it matches the settings for the first trail\. The trail logs and delivers the event to the `read-only-bucket`\.

1. The `TerminateInstances` API operation is a write\-only event and it matches the settings for the second trail\. The trail logs and delivers the event to the `write-only-bucket`\.

## Logging data events with the AWS Command Line Interface<a name="creating-data-event-selectors-with-the-AWS-CLI"></a>

You can configure your trails or event data stores to log data events using the AWS CLI\.

**Topics**
+ [Examples: Logging data events for trails](#logging-data-events-CLI-trail-examples)
+ [Examples: Logging data events for event data stores](#logging-data-events-CLI-eds-examples)

### Examples: Logging data events for trails<a name="logging-data-events-CLI-trail-examples"></a>

You can configure your trails to log management and data events using the AWS CLI\. To see whether your trail is logging management and data events, run the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/get-event-selectors.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/get-event-selectors.html) command\. 

**Note**  
Be aware that if your account is logging more than one copy of management events, you incur charges\. There is always a charge for logging data events\. For more information, see [AWS CloudTrail Pricing](http://aws.amazon.com/cloudtrail/pricing/)\.

```
aws cloudtrail get-event-selectors --trail-name TrailName
```

The command returns the default settings for a trail\.

**Topics**
+ [Log events by using basic event selectors](#creating-data-event-selectors-basic)
+ [Log events by using advanced event selectors](#creating-data-event-selectors-advanced)
+ [Log all Amazon S3 events for a bucket by using advanced event selectors](#creating-data-adv-event-selectors-CLI-s3)
+ [Log Amazon S3 on AWS Outposts events by using advanced event selectors](#creating-data-event-selectors-CLI-outposts)
+ [Log all data events by using advanced event selectors](#creating-data-adv-event-selectors-CLI-all-data)

#### Log events by using basic event selectors<a name="creating-data-event-selectors-basic"></a>

The following is an example result of the get\-event\-selectors command showing basic event selectors\. By default, when you create a trail by using the AWS CLI, a trail logs all management events\. By default, trails do not log data events\.

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

To configure your trail to log management and data events, run the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/put-event-selectors.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/put-event-selectors.html) command\.

The following example shows how to use basic event selectors to configure your trail to include all management and data events for two S3 objects\. You can specify from 1 to 5 event selectors for a trail\. You can specify from 1 to 250 data resources for a trail\.

**Note**  
The maximum number of S3 data resources is 250, if you choose to limit data events by using basic event selectors\.

```
aws cloudtrail put-event-selectors --trail-name TrailName --event-selectors '[{ "ReadWriteType": "All", "IncludeManagementEvents":true, "DataResources": [{ "Type": "AWS::S3::Object", "Values": ["arn:aws:s3:::mybucket/prefix", "arn:aws:s3:::mybucket2/prefix2"] }] }]'
```

The command returns the event selectors that are configured for the trail\.

```
{
    "EventSelectors": [
        {
            "IncludeManagementEvents": true,
            "DataResources": [
                {
                    "Values": [
                        "arn:aws:s3:::mybucket/prefix",
                        "arn:aws:s3:::mybucket2/prefix2",
                    ],
                    "Type": "AWS::S3::Object"
                }
            ],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

#### Log events by using advanced event selectors<a name="creating-data-event-selectors-advanced"></a>

If you have opted to use advanced event selectors, the get\-event\-selectors command returns results similar to the following\. By default, no advanced event selectors are configured for a trail\.

```
{
    "AdvancedEventSelectors": [],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```



The following example shows how to use advanced event selectors to log all management events \(both `readOnly` and `writeOnly`\), and include `PutObject` and `DeleteObject` events for the S3 objects in the same two S3 bucket prefixes\. As shown here, you can use advanced event selectors to select not only the S3 prefix names by ARN, but the names of the specific events that you want to log\. You can add up to 500 conditions to advanced event selectors per trail, including all selector values\. You can specify from 1 to 250 data resources for a trail\.

```
aws cloudtrail put-event-selectors --trail-name TrailName \
--advanced-event-selectors 
'[
  {
    "Name": "Log readOnly and writeOnly management events",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Management"] }
    ]
  },
  {
    "Name": "Log PutObject and DeleteObject events for two S3 prefixes",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
      { "Field": "eventName", "Equals": ["PutObject","DeleteObject"] },
      { "Field": "resources.ARN", "StartsWith": ["arn:aws:s3:::mybucket/prefix","arn:aws:s3:::mybucket2/prefix2"] }
    ]
  }
]'
```

The result shows the advanced event selectors that are configured for the trail\.

```
{
  "AdvancedEventSelectors": [
    {
      "Name": "Log readOnly and writeOnly management events",
      "FieldSelectors": [
        {
          "Field": "eventCategory", 
          "Equals": [ "Management" ],
          "StartsWith": [],
          "EndsWith": [],
          "NotEquals": [],
          "NotStartsWith": [],
          "NotEndsWith": []
        }
      ]
    },
    {
      "Name": "Log PutObject and DeleteObject events for two S3 prefixes",
      "FieldSelectors": [
        {
          "Field": "eventCategory", 
          "Equals": [ "Data" ],
          "StartsWith": [],
          "EndsWith": [],
          "NotEquals": [],
          "NotStartsWith": [],
          "NotEndsWith": []
        },
        {
          "Field": "resources.type", 
          "Equals": [ "AWS::S3::Object" ],
          "StartsWith": [],
          "EndsWith": [],
          "NotEquals": [],
          "NotStartsWith": [],
          "NotEndsWith": []
        },
        {
          "Field": "resources.ARN", 
          "Equals": [],
          "StartsWith": [ "arn:aws:s3:::mybucket/prefix","arn:aws:s3:::mybucket2/prefix2" ],
          "EndsWith": [],
          "Equals": [],
          "NotStartsWith": [],
          "NotEndsWith": []
        }
      ]
    }
  ],
  "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

#### Log all Amazon S3 events for a bucket by using advanced event selectors<a name="creating-data-adv-event-selectors-CLI-s3"></a>

The following example shows how to configure your trail to include all data events for all Amazon S3 objects in a specific S3 bucket\. The value for S3 events for the `resources.type` field is `AWS::S3::Object`\. Because the ARN values for S3 objects and S3 buckets are slightly different, you must add the `StartsWith` operator for `resources.ARN` to capture all events\.

```
aws cloudtrail put-event-selectors --trail-name TrailName --region region \
--advanced-event-selectors \
'[
    {
            "Name": "S3EventSelector",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
                { "Field": "resources.ARN", "StartsWith": ["arn:partition:s3:::bucket_name/"] }
            ]
        }
]'
```

The command returns the following example output\.

```
{
    "AdvancedEventSelectors": [
        {
            "Name": "S3EventSelector",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3::Object"
                    ]
                },
                {
                    "Field": "resources.ARN",
                    "StartsWith": [
                        "arn:partition:s3:::bucket_name/"
                    ]
                }
            ]
        }
    ],
  "TrailARN": "arn:aws:cloudtrail:region:account_ID:trail/TrailName"
}
```

#### Log Amazon S3 on AWS Outposts events by using advanced event selectors<a name="creating-data-event-selectors-CLI-outposts"></a>

The following example shows how to configure your trail to include all data events for all Amazon S3 on Outposts objects in your outpost\.

```
aws cloudtrail put-event-selectors --trail-name TrailName --region region \
--advanced-event-selectors \
'[
    {
            "Name": "OutpostsEventSelector",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3Outposts::Object"] }
            ]
        }
]'
```

The command returns the following example output\.

```
{
    "AdvancedEventSelectors": [
        {
            "Name": "OutpostsEventSelector",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3Outposts::Object"
                    ]
                }
            ]
        }
    ],
  "TrailARN": "arn:aws:cloudtrail:region:account_ID:trail/TrailName"
}
```

#### Log all data events by using advanced event selectors<a name="creating-data-adv-event-selectors-CLI-all-data"></a>

The following example shows how to configure your trail to include data events for all S3 buckets, Lambda functions, DynamoDB tables, S3 object\-level API activity on AWS Outposts, Amazon Managed Blockchain JSON\-RPC calls on Ethereum nodes, API activity on S3 Object Lambda access points, Amazon EBS direct API activity on Amazon EBS snapshots in the AWS account, S3 access points, DynamoDB streams, AWS Glue tables created by Lake Formation, Amazon FinSpace environments, Amazon SageMaker metrics experiment trial components, Amazon SageMaker feature stores, Amazon Cognito identity pools, Amazon Kendra Intelligent Ranking rescore execution plans, Amazon EC2 Instance Connect endpoints, and Amazon GuardDuty detectors\.

**Note**  
If the trail applies only to one Region, only events in that Region are logged, even though the event selector parameters specify all Amazon S3 buckets and Lambda functions\. In a single\-region trail, event selectors apply only to the Region where the trail is created\.

```
aws cloudtrail put-event-selectors --trail-name TrailName \
--advanced-event-selectors '
[
  {
    "Name": "Log all events for all Amazon S3 buckets",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::S3::Object"] }
    ]
  },
  {
    "Name": "Log all events for Lambda functions",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::Lambda::Function"] }
    ]
  },
  {
    "Name": "Log all events for DynamoDB tables",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::DynamoDB::Table"] }
    ]
  },
  {
    "Name": "Log all CloudTrail PutAuditEvents activity on a CloudTrail Lake channel,
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::CloudTrail::Channel"] }
    ]
  },
  {
    "Name": "Log all events for Amazon S3 on Outposts",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::S3Outposts::Object"] }
    ]
  },
  {
    "Name": "Log all JSON-RPC calls for Ethereum nodes in Amazon Managed Blockchain",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::ManagedBlockchain::Node"] }
    ]
  },
  {
    "Name": "Log all events for Amazon S3 Object Lambda access points",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::S3ObjectLambda::AccessPoint"] }
    ]
  },
  {
    "Name": "Log all Amazon EBS direct API calls on snapshots",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::EC2::Snapshot"] }
    ]
  },
  {
    "Name": "Log all events for Amazon S3 access points",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::S3::AccessPoint"] }
    ]
  },
  {
    "Name": "Log all events for DynamoDB streams",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::DynamoDB::Stream"] }
    ]
  },
  {
    "Name": "Log all events for AWS Glue tables created by Lake Formation",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::Glue::Table"] }
    ]
  },
  {
    "Name": "Log all events for FinSpace environments",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::FinSpace::Environment"] }
    ]
  },
  {
    "Name": "Log all events for SageMaker metrics experiment trial components",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::SageMaker::ExperimentTrialComponent"] }
    ]
  },
  {
    "Name": "Log all events for SageMaker feature stores",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::SageMaker::FeatureGroup"] }
    ]
  },
  {
    "Name": "Log all events for Amazon Kendra Intelligent Ranking rescore execution plans",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::KendraRanking::ExecutionPlan"] }
    ]
  },
  {
    "Name": "Log all events for Amazon Cognito identity pools",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::Cognito::IdentityPool"] }
    ]
  },
  {
    "Name": "Log all events for Amazon EC2 Instance Connect endpoints",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::EC2::InstanceConnectEndpoint"] }
    ]
  },
  {
    "Name": "Log all events for Amazon GuardDuty detectors",
    "FieldSelectors": [
      { "Field": "eventCategory", "Equals": ["Data"] },
      { "Field": "resources.type", "Equals": ["AWS::GuardDuty::Detector"] }
    ]
  }
]'
```

The command returns the following example output\.

```
{
    "TrailARN": "arn:aws:cloudtrail:us-east-1:111122223333:trail/TrailName",
    "AdvancedEventSelectors": [
        {
            "Name": "Log all events for all Amazon S3 buckets",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3::Object"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Lambda functions",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::Lambda::Function"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for DynamoDB tables",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::DynamoDB::Table"
                    ]
                }
            ]
        },
        {
            "Name": "Log all CloudTrail PutAuditEvents activity on a CloudTrail Lake channel",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::CloudTrail::Channel"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon S3 on Outposts",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3Outposts::Object"
                    ]
                }
            ]
        },
        {
            "Name": "Log all JSON-RPC calls for Ethereum nodes in Amazon Managed Blockchain",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::ManagedBlockchain::Node"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon S3 Object Lambda access points",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3ObjectLambda::AccessPoint"
                    ]
                }
            ]
        },
        {
            "Name": "Log all Amazon EBS direct API calls on snapshots",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::EC2::Snapshot"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon S3 access points",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3::AccessPoint"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for DynamoDB streams",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::DynamoDB::Stream"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for AWS Glue tables created by Lake Formation",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::Glue::Table"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for FinSpace environments",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::FinSpace::Environment"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for SageMaker metrics experiment trial components",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::SageMaker::ExperimentTrialComponent"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for SageMaker feature stores",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::SageMaker::FeatureGroup"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon Kendra Intelligent Ranking rescore execution plans",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::KendraRanking::ExecutionPlan"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon Cognito identity pools",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::Cognito::IdentityPool"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon EC2 Instance Connect endpoints",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::EC2::InstanceConnectEndpoint"
                    ]
                }
            ]
        },
        {
            "Name": "Log all events for Amazon GuardDuty detectors",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::GuardDuty::Detector"
                    ]
                }
            ]
        }
    ]
}
```

### Examples: Logging data events for event data stores<a name="logging-data-events-CLI-eds-examples"></a>

You can configure your event data stores to include data events using the AWS CLI\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/create-event-data-store.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/create-event-data-store.html) command to create a new event data store to log data events\. Use the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/update-event-data-store.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/update-event-data-store.html) command to update the advanced event selectors for an existing event data store\.

To see whether your event data store includes data events, run the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/get-event-data-store.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/get-event-data-store.html) command\. 

```
aws cloudtrail get-event-data-store --event-data-store EventDataStoreARN
```

The command returns the settings for the event data store\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:111122223333:eventdatastore/EXAMPLE492-301f-4053-ac5e-EXAMPLE6441aa",
    "Name": "ebs-data-events",
    "Status": "ENABLED",
    "AdvancedEventSelectors": [
        {
            "Name": "Log all EBS direct APIs on EBS snapshots",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::EC2::Snapshot"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 2557,
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2023-01-04T15:57:33.701000+00:00",
    "UpdatedTimestamp": "2023-02-20T20:37:34.228000+00:00"
}
```

**Topics**
+ [Include all Amazon S3 events for a bucket](#creating-data-adv-event-selectors-CLI-s3-eds)
+ [Include Amazon S3 on AWS Outposts events](#creating-data-event-selectors-CLI-outposts-eds)

#### Include all Amazon S3 events for a bucket<a name="creating-data-adv-event-selectors-CLI-s3-eds"></a>

The following example shows how to create an event data store to include all data events for all Amazon S3 objects in a specific S3 bucket\. The value for S3 events for the `resources.type` field is `AWS::S3::Object`\. Because the ARN values for S3 objects and S3 buckets are slightly different, you must add the `StartsWith` operator for `resources.ARN` to capture all events\.

```
aws cloudtrail create-event-data-store --name "EventDataStoreName" --multi-region-enabled \
--advanced-event-selectors \
'[
    {
            "Name": "S3EventSelector",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
                { "Field": "resources.ARN", "StartsWith": ["arn:partition:s3:::bucket_name/"] }
            ]
        }
]'
```

The command returns the following example output\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:111122223333:eventdatastore/EXAMPLE492-301f-4053-ac5e-EXAMPLE441aa",
    "Name": "EventDataStoreName",
    "Status": "ENABLED",
    "AdvancedEventSelectors": [
        {
            "Name": "S3EventSelector",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.ARN",
                    "StartsWith": [
                        "arn:partition:s3:::bucket_name/"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3::Object"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 2557,
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2023-01-04T15:57:33.701000+00:00",
    "UpdatedTimestamp": "2023-02-20T20:49:21.766000+00:00"
}
```

#### Include Amazon S3 on AWS Outposts events<a name="creating-data-event-selectors-CLI-outposts-eds"></a>

The following example shows how to create an event data store that includes all data events for all Amazon S3 on Outposts objects in your outpost\.

```
aws cloudtrail create-event-data-store --name EventDataStoreName \
--advanced-event-selectors \
'[
    {
            "Name": "OutpostsEventSelector",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3Outposts::Object"] }
            ]
        }
]'
```

The command returns the following example output\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:111122223333:eventdatastore/EXAMPLEb4a8-99b1-4ec2-9258-EXAMPLEc890",
    "Name": "EventDataStoreName",
    "Status": "CREATED",
    "AdvancedEventSelectors": [
        {
            "Name": "OutpostsEventSelector",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3Outposts::Object"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 2557,
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2023-02-20T21:00:17.673000+00:00",
    "UpdatedTimestamp": "2023-02-20T21:00:17.820000+00:00"
}
```

## Logging data events for AWS Config compliance<a name="config-data-events-best-practices"></a>

If you are using AWS Config conformance packs to help your enterprise maintain compliance with formalized standards such as those required by Federal Risk and Authorization Management Program \(FedRAMP\) or National Institute of Standards and Technology \(NIST\), conformance packs for compliance frameworks generally require you to log data events for Amazon S3 buckets, at minimum\. Conformance packs for compliance frameworks include a [managed rule](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_use-managed-rules.html) called [https://docs.aws.amazon.com/config/latest/developerguide/cloudtrail-s3-dataevents-enabled.html](https://docs.aws.amazon.com/config/latest/developerguide/cloudtrail-s3-dataevents-enabled.html) that checks for S3 data event logging in your account\. Many conformance packs that are not associated with compliance frameworks also require S3 data event logging\. The following are examples of conformance packs that include this rule\.
+ [Operational Best Practices for AWS Well\-Architected Framework Security Pillar](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-wa-Security-Pillar.html)
+ [Operational Best Practices for FDA Title 21 CFR Part 11](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-FDA-21CFR-Part-11.html)
+ [Operational Best Practices for FFIEC](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-ffiec.html)
+ [Operational Best Practices for FedRAMP\(Moderate\)](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-ffiec.html)
+ [Operational Best Practices for HIPAA Security](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-hipaa_security.html)
+ [Operational Best Practices for K\-ISMS](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-k-isms.html)
+ [Operational Best Practices for Logging](https://docs.aws.amazon.com/config/latest/developerguide/operational-best-practices-for-logging.html)

For a full list of sample conformance packs available in AWS Config, see [Conformance pack sample templates](https://docs.aws.amazon.com/config/latest/developerguide/conformancepack-sample-templates.html) in the *AWS Config Developer Guide\.*

## Logging events with the AWS SDKs<a name="logging-data-events-with-the-AWS-SDKs"></a>

Run the [GetEventSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_GetEventSelectors.html) operation to see whether your trail is logging data events\. You can configure your trails to log data events by running the [PutEventSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutEventSelectors.html) operation\. For more information, see the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

Run the [GetEventDataStore](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_GetEventDataStore.html) operation to see whether your event data store is logging data events\. You can configure your event data stores to include data events by running the [CreateEventDataStore](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateEventDataStore.html) or [UpdateEventDataStore](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_UpdateEventDataStore.html) operations and specifying advanced event selectors\. For more information, see [Managing CloudTrail Lake by using the AWS CLI](query-lake-cli.md) and the [AWS CloudTrail API Reference](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.

## Sending events to Amazon CloudWatch Logs<a name="sending-data-events-to-cloudwatch-logs"></a>

CloudTrail supports sending data events to CloudWatch Logs\. When you configure your trail to send events to your CloudWatch Logs log group, CloudTrail sends only the events that you specify in your trail\. For example, if you configure your trail to log data events only, your trail delivers data events only to your CloudWatch Logs log group\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\. 