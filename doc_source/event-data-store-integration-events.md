# Create an event data store for events outside of AWS<a name="event-data-store-integration-events"></a>

**Note**  
Currently, integrations are supported in all commercial AWS Regions supported by CloudTrail Lake except: me\-central\-1\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

You can create an event data store to include events outside of AWS, and then use CloudTrail Lake to search, query, and analyze the data that is logged from your applications\.

You can use CloudTrail Lake *integrations* to log and store user activity data from outside of AWS; from any source in your hybrid environments, such as in\-house or SaaS applications hosted on\-premises or in the cloud, virtual machines, or containers\.

When you create an event data store for an integration, you also create a channel, and attach a resource policy to the channel\. 

## To create an event data store for events outside of AWS<a name="event-data-store-integration-events-procedure"></a>

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1.  From the navigation pane, open the **Lake** submenu, then choose **Event data stores**\. 

1. Choose **Create event data store**\.

1. On the **Configure event data store** page, in **General details**, enter a name for the event data store\. A name is required\.

1. Specify a retention period for the event data store in days\. Valid values are integers between 7 to 2557 \(seven years\)\. The event data store retains event data for the specified number of days\. By default, event data is retained for 2557 days\.

1. \(Optional\) To enable encryption using AWS Key Management Service, choose **Use my own AWS KMS key**\. Choose **New** to have an AWS KMS key created for you, or choose **Existing** to use an existing KMS key\. In **Enter KMS alias**, specify an alias, in the format `alias/`*MyAliasName*\. Using your own KMS key requires that you edit your KMS key policy to allow CloudTrail logs to be encrypted and decrypted\. For more information, see [Configure AWS KMS key policies for CloudTrail](create-kms-key-policy-for-cloudtrail.md)\. CloudTrail also supports AWS KMS multi\-Region keys\. For more information about multi\-Region keys, see [Using multi\-Region keys](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-overview.html) in the *AWS Key Management Service Developer Guide*\.

   Using your own KMS key incurs AWS KMS costs for encryption and decryption\. After you associate an event data store with a KMS key, the KMS key cannot be removed or changed\.
**Note**  
To enable AWS Key Management Service encryption for an organization event data store, you must use an existing KMS key for the management account\.

1. \(Optional\) In the **Tags** section, you can add up to 50 tag key pairs to help you identify, sort, and control access to your event data store\. For more information about how to use IAM policies to authorize access to an event data store based on tags, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags)\. For more information about how you can use tags in AWS, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.

1.  Choose **Next** to configure the event data store\. 

1.  On the **Choose events** page, choose **Events from integrations**\. 

1.  From **Events from integration**, choose the source to deliver events to the event data store\. 

1. Provide a name to identify the integration's channel\. The name can be 3\-128 characters\. Only letters, numbers, periods, underscores, and dashes are allowed\.

1. In **Resource policy**, configure the resource policy for the integration's channel\. Resource policies are JSON policy documents that specify what actions a specified principal can perform on the resource and under what conditions\. The accounts defined as principals in the resource policy can call the `PutAuditEvents` API to deliver events to your channel\. The resource owner has implicit access to the resource if their IAM policy allows the `cloudtrail-data:PutAuditEvents` action\.

   The information required for the policy is determined by the integration type\. For a direction integration, CloudTrail automatically adds the partner's AWS account IDs, and requires you to enter the unique external ID provided by the partner\. For a solution integration, you must specify at least one AWS account ID as principal, and can optionally enter an external ID to prevent against confused deputy\.
**Note**  
If you do not create a resource policy for the channel, only the channel owner can call the `PutAuditEvents` API on the channel\.

   1. For a direct integration, enter the external ID provided by your partner\. The integration partner provides a unique external ID, such as an account ID or a randomly generated string, to use for the integration to prevent against confused deputy\. The partner is responsible for creating and providing a unique external ID\.

       You can choose **How to find this?** to view the partner's documentation that describes how to find the external ID\.   
![\[Partner documentation for external ID\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/integration-external-id.png)
**Note**  
If the resource policy includes an external ID, all calls to the `PutAuditEvents` API must include the external ID\. However, if the policy does not define an external ID, the partner can still call the `PutAuditEvents` API and specify an `externalId` parameter\.

   1.  For a solution integration, choose **Add AWS account** to specify each AWS account ID to add as a principal in the policy\.

1. Choose **Next** to review your choices\.

1. On the **Review and create** page, review your choices\. Choose **Edit** to make changes to a section\. When you're ready to create the event data store, choose **Create event data store**\.

1. The new event data store is visible in the **Event data stores** table on the **Lake** page\.

1. Provide the channel Amazon Resource Name \(ARN\) to the partner application\. Instructions for providing the channel ARN to the partner application are found on the partner documentation website\. For more information, choose the **Learn more** link for the partner on the **Available sources** tab of the **Integrations** page to open the partner's page in AWS Marketplace\.

The event data store starts ingesting partner events into CloudTrail through the integration's channel when you, the partner, or the partner applications calls the `PutAuditEvents` API on the channel\.