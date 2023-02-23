# Create an event data store for CloudTrail events<a name="query-event-data-store-cloudtrail"></a>

Event data stores for CloudTrail events can log CloudTrail management and data events\. You can keep the event data in an event data store for up to seven years, or 2557 days\. By default, event data is retained for 2557 days, and termination protection is enabled for an event data store\.

## To create an event data store for CloudTrail events<a name="query-event-data-store-cloudtrail-procedure"></a>

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

1.  On the **Choose events** page, choose **AWS events**, and then choose **CloudTrail events**\. 

1. For **CloudTrail events**, choose at least one event type\. By default, **Management events** is selected\. You can add both management and data events to your event data store\. For more information about management events, see [Logging management events](logging-management-events-with-cloudtrail.md)\. For more information about data events, see [Logging data events](logging-data-events-with-cloudtrail.md)\.

1. \(Optional\) Choose **Copy trail events** if you want to copy events from an existing trail to run queries on past events\. To copy trail events to an organization event data store, you must use the management account for the organization\. The delegated administrator account cannot copy trail events to an organization event data store\. For more information about considerations for copying trail events, see [Considerations](cloudtrail-copy-trail-to-lake-eds.md#cloudtrail-trail-copy-considerations-lake)\.

1. CloudTrail stores the event data store resource in the AWS Region in which you create it, but by default, the events collected in the data store are from all Regions in your account\. Optionally, you can select **Include only the current region in my event data store** to include only events that are logged in the current Region\. If you do not choose this option, your event data store includes events from all Regions\.

1. To have your event data store collect events from all accounts in an AWS Organizations organization, select **Enable for all accounts in my organization**\. You must be signed in to the management account or delegated administrator account for the organization to create an event data store that collects events for an organization\.

1. If your event data store includes management events, choose **Read**, **Write**, or both\. At least one is required\. For more information about **Read** and **Write** management events, see [Logging management events](logging-management-events-with-cloudtrail.md)\.

1. You can choose to exclude AWS Key Management Service or Amazon RDS Data API events from your event data store\. For more information about these options, see [Logging management events](logging-management-events-with-cloudtrail.md)\.

1. To include data events in your event data store, do the following\.

   1. Choose a data event type\. This is the AWS service and resource on which data events are logged\. To log data events for AWS Glue tables created by Lake Formation, choose **Lake Formation** for the data type\.

   1. In **Log selector template**, choose a template\. You can choose to log all data events, `readOnly` events, `writeOnly` events, or **Custom** to build a custom log selector\.

   1. If you choose **Custom**, optionally enter a name for your custom log selector template\.

   1. In **Advanced event selectors**, build expressions by choosing values for **Field**, **Operator**, and **Value**\. Advanced event selectors for an event data store work the same as advanced event selectors that you apply to a trail\. For more information about how to build advanced event selectors, see [Logging data events with advanced event selectors](logging-data-events-with-cloudtrail.md#creating-data-event-selectors-advanced)\.

      The following example uses a **Custom** log selector template to choose only event names from S3 objects that start with `Put`, such as `PutObject`\. Because the advanced event selector does not include or exclude any other event types or resource ARNs, all S3 data events, both read and write, that are logged to the US East \(N\. Virginia\) Region, and that have event names starting with `Put`, are stored in the event data store\.  
![\[Lake data events, advanced event selectors\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-data-events.png)
**Important**  
To exclude or include data events with advanced event selectors by using an S3 bucket ARN, always use the **Starts with** operator\.

   1. Optionally, expand **JSON view** to see your advanced event selectors as a JSON block\.

1. To copy existing trail events to your event data store, do the following\.

   1. Choose the trail that you want to copy\. By default, CloudTrail only copies CloudTrail events contained in the S3 bucket's `CloudTrail` prefix and the prefixes inside the `CloudTrail` prefix, and does not check prefixes for other AWS services\. If you want to copy CloudTrail events contained in another prefix, choose **Enter S3 URI**, and then choose **Browse S3** to browse to the prefix\. If the source S3 bucket for the trail uses a KMS key for data encryption, ensure that the KMS key policy allows CloudTrail to decrypt the data\. If your source S3 bucket uses multiple KMS keys, you must update each key's policy to allow CloudTrail to decrypt the data in the bucket\. For more information about updating the KMS key policy, see [KMS key policy for decrypting data in the source S3 bucket](cloudtrail-copy-trail-to-lake-eds.md#copy-trail-events-permissions-kms)\.

   1. \(Optional\) Choose a time range for copying the events\. If you choose a time range, CloudTrail checks the prefix and log file name to verify the name contains a date between the chosen start and end date before attempting to copy trail events\. You can choose a **Relative range** or an **Absolute range**\. To avoid duplicating events between the source trail and destination event data store, choose a time range that is earlier than the creation of the event data store\.
      + If you choose **Relative range**, you can choose to copy events logged in the last 5 minutes, 30 minutes, 1 hour, 6 hours, or a custom range\. CloudTrail copies the events logged within the chosen time period\.
      + If you choose **Absolute range**, you can choose a specific start and end date\. CloudTrail copies the events that occurred between the chosen start and end dates\.

   1. For **Permissions**, choose from the following IAM role options\. If you choose an existing IAM role, verify that the IAM role policy provides the necessary permissions\. For more information about updating the IAM role permissions, see [IAM permissions for copying trail events](cloudtrail-copy-trail-to-lake-eds.md#copy-trail-events-permissions-iam)\.
      + Choose **Create a new role \(recommended\)** to create a new IAM role\. For **Enter IAM role name**, enter a name for the role\. CloudTrail automatically creates the necessary permissions for this new role\.
      + Choose **Use a custom IAM role** to use a custom IAM role that is not listed\. For **Enter IAM role ARN**, enter the IAM ARN\.
      + Choose **Use an existing role** to choose an existing IAM role from the drop\-down list\.

1. Choose **Next** to review your choices\.

1. On the **Review and create** page, review your choices\. Choose **Edit** to make changes to a section\. When you're ready to create the event data store, choose **Create event data store**\.

1. The new event data store is visible in the **Event data stores** table on the **Lake** page\.

   From this point forward, the event data store captures events that match its advanced event selectors\. Events that occurred before you created the event data store are not in the event data store, unless you opted to copy existing trail events\.

You can now run queries on your new event data store\. The **Sample queries** tab provides example queries to get you started\. For more information about creating and editing queries, see [Create or edit a query](query-create-edit-query.md)\.