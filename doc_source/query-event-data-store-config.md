# Create an event data store for AWS Config configuration items<a name="query-event-data-store-config"></a>

You can create an event data store to include AWS Config configuration items, and use the event data store to investigate non\-compliant changes to your production environments\. With an event data store, you can relate non\-compliant rules to the users and resources associated with the changes\. A configuration item represents a point\-in\-time view of the attributes of a supported AWS resource that exists in your account\. AWS Config creates a configuration item whenever it detects a change to a resource type that it is recording\. AWS Config also creates configuration items when a configuration snapshot is captured\. 

You can use both AWS Config and CloudTrail Lake to run queries against your configuration items\. You can use AWS Config to query the current configuration state of AWS resources based on configuration properties for a single AWS account and AWS Region, or across multiple accounts and Regions\. In contrast, you can use CloudTrail Lake to query across diverse data sources such as CloudTrail events, configuration items, and rule evaluations\. CloudTrail Lake queries cover all AWS Config configuration items including resource configuration and compliance history\.

Creating an event data store for configuration items doesn't impact existing AWS Config advanced queries, or any configured AWS Config aggregators\. You can continue to run advanced queries using AWS Config, and AWS Config continues to deliver history files to your S3 buckets\.

## Limitations<a name="query-event-data-store-config-limitations"></a>

The following limitations apply to event data stores for configuration items\.
+ No support for custom configuration items
+ No support for event filtering using advanced event selectors

## Prerequisites<a name="query-event-data-store-config-prerequisites"></a>

Before you create your event data store, set up AWS Config recording for all your accounts and Regions\. You can use [Quick Setup](https://docs.aws.amazon.com/systems-manager/latest/userguide/quick-setup-config.html), a capability of AWS Systems Manager, to quickly create a configuration recorder powered by AWS Config\. 

**Note**  
You are charged service usage fees when AWS Config starts recording configurations\. For more information about pricing, see [AWS Config Pricing](http://aws.amazon.com/)\. For information about managing the configuration recorder, see [Managing the Configuration Recorder](https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html) in the *AWS Config Developer Guide*\.  


Additionally, the following actions are recommended, but are not required to create an event data store\.
+  Set up an Amazon S3 bucket to receive a configuration snapshot on request and configuration history\. For more information about snapshots, see [Managing the Delivery Channel](https://docs.aws.amazon.com/config/latest/developerguide/manage-delivery-channel.html) and [Delivering Configuration Snapshot to an Amazon S3 Bucket](https://docs.aws.amazon.com/config/latest/developerguide/deliver-snapshot-cli.html) in the *AWS Config Developer Guide*\. 
+  Specify the rules that you want AWS Config to use to evaluate compliance information for the recorded resource types\. Several of the CloudTrail Lake sample queries for AWS Config require AWS Config Rules to evaluate the compliance state of your AWS resources\. For more information about AWS Config Rules, see [Evaluating Resources with AWS Config Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html) in the *AWS Config Developer Guide*\. 

## To create an event data store for configuration items<a name="create-config-event-data-store"></a>

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Lake** in the left navigation pane of the CloudTrail console\.

1. On the **Lake** page, choose the **Event data stores** tab\.

1. Choose **Create event data store**\.

1. On the **Configure event data store** page, in **General details**, enter a name for the event data store\. A name is required\.

1. Specify a retention period for the event data store in days\. Valid values are integers from 7 to 2557 \(seven years\)\. The event data store retains configuration items for the specified number of days\. By default, event data is retained for 2557 days\.

1. \(Optional\) To enable encryption using AWS Key Management Service, choose **Use my own AWS KMS key**\. Choose **New** to have an AWS KMS key created for you, or choose **Existing** to use an existing KMS key\. In **Enter KMS alias**, specify an alias, in the format `alias/`*MyAliasName*\. Using your own KMS key requires that you edit your KMS key policy to allow CloudTrail logs to be encrypted and decrypted\. For more information, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\. CloudTrail also supports AWS KMS multi\-Region keys\. For more information about multi\-Region keys, see [Using multi\-Region keys](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html) in the *AWS Key Management Service Developer Guide*\.

   Using your own KMS key incurs AWS KMS costs for encryption and decryption\. After you associate an event data store with a KMS key, the KMS key cannot be removed or changed\.
**Note**  
To enable AWS Key Management Service encryption for an organization event data store, you must use an existing KMS key for the management account\.

1. \(Optional\) In the **Tags** section, you can add up to 50 tag key pairs to help you identify, sort, and control access to your event data store\. For more information about how to use IAM policies to authorize access to an event data store based on tags, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags)\. For more information about how you can use tags in AWS, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.

1. Choose **Next**\.

1. On the **Choose events** page, choose **Configuration items**\.

1. CloudTrail stores the event data store resource in the Region in which you create it, but by default, the configuration items collected in the data store are from all Regions in your account that have recording enabled\. Optionally, you can select **Include only the current region in my event data store** to include only configuration items that are captured in the current Region\. If you do not choose this option, your event data store includes configuration items from all Regions that have recording enabled\.

1. To have your event data store collect configuration items from all accounts in an AWS Organizations organization, select **Enable for all accounts in my organization**\. You must be signed in to the management account or delegated administrator account for the organization to create an event data store that collects configuration items for an organization\.

1. Choose **Next** to review your choices\.

1. On the **Review and create** page, review your choices\. Choose **Edit** to make changes to a section\. When you're ready to create the event data store, choose **Create event data store**\.

1. The new event data store is visible in the **Event data stores** table on the **Lake** page\.

   From this point forward, the event data store captures configuration items\. Configuration items that occurred before you created the event data store are not in the event data store\.

### Sample queries<a name="query-event-data-store-config-queries"></a>

You can now run queries on your new event data store\. The **Sample queries** tab on the CloudTrail console provides example queries to get you started\. The following are a few of the sample queries that you can run against your configuration item event data store\.


| Description | Query | 
| --- | --- | 
| Find which user performed an action that resulted in a non\-compliant status by joining a configuration item event data store with a CloudTrail event data store\. |  <pre>SELECT<br />    element_at(config1.eventData.configuration, 'targetResourceId') as targetResourceId,<br />    element_at(config1.eventData.configuration, 'complianceType') as complianceType,<br />    config2.eventData.resourceType, cloudtrail.userIdentity<br />FROM<br />    config_event_data_store_ID  as config1<br />JOIN<br />    config_event_data_store_ID  as config2 on element_at(config1.eventData.configuration, 'targetResourceId') = config2.eventData.resourceId<br />JOIN<br />    cloudtrail_event_data_store_ID  as cloudtrail on config2.eventData.arn = element_at(cloudtrail.resources, 1).arn<br />WHERE<br />    element_at(config1.eventData.configuration, 'configRuleList') is not null<br />AND <br />    element_at(config1.eventData.configuration, 'complianceType') = 'NON_COMPLIANT'<br />AND <br />    cloudtrail.eventTime > '2022-11-14 00:00:00'<br />AND <br />    config2.eventData.resourceType = 'AWS::DynamoDB::Table'<br />                                </pre>  | 
| Find all AWS Config rules and return the compliance state from configuration items generated within the past day\. |  <pre>SELECT <br />    eventData.configuration, eventData.accountId, eventData.awsRegion, <br />    eventData.resourceName, eventData.resourceCreationTime, <br />    element_at(eventData.configuration,'complianceType') AS complianceType, <br />    element_at(eventData.configuration, 'configRuleList') AS configRuleList, <br />    element_at(eventData.configuration, 'resourceId') AS resourceId, <br />    element_at(eventData.configuration, 'resourceType') AS resourceType <br />FROM <br />    config_event_data_store_ID <br />WHERE <br />    eventData.resourceType = 'AWS::Config::ResourceCompliance' <br />AND <br />    eventTime > '2022-11-22 00:00:00' <br />ORDER BY <br />    eventData.resourceCreationTime <br />DESC <br />    limit 10<br />                                </pre>  | 
| Find the total count of AWS Config resources grouped by resource type, account ID, and Region\. |  <pre>SELECT <br />    eventData.resourceType, eventData.awsRegion, eventData.accountId, <br />COUNT (*) AS resourceCount <br />FROM <br />    config_event_data_store_ID <br />WHERE <br />    eventTime > '2022-11-22 00:00:00' <br />GROUP BY <br />    eventData.resourceType, eventData.awsRegion, eventData.accountId<br />                                </pre>  | 
| Find the resource creation time for all AWS Config configuration items generated on a specific date\. |  <pre>SELECT<br />    eventData.configuration, eventData.accountId, <br />    eventData.awsRegion, eventData.resourceId, <br />    eventData.resourceName, eventData.resourceType, <br />    eventData.availabilityZone, eventData.resourceCreationTime<br />FROM <br />    config_event_data_store_ID<br />WHERE <br />    eventTime > '2022-11-16 00:00:00' <br />AND <br />    eventTime < '2022-11-17 00:00:00'  <br />ORDER BY <br />    eventData.resourceCreationTime<br />DESC <br />    limit 10;                             </pre>  | 

For more information about creating and editing queries, see [Create or edit a query](query-create-edit-query.md)\.