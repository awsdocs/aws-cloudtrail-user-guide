# Working with AWS CloudTrail Lake<a name="cloudtrail-lake"></a>

AWS CloudTrail Lake lets you run SQL\-based queries on your events\. CloudTrail Lake converts existing events in row\-based JSON format to [ Apache ORC](https://orc.apache.org/) format\. ORC is a columnar storage format that is optimized for fast retrieval of data\. Events are aggregated into event data stores, which are immutable collections of events based on criteria that you select by applying [advanced event selectors](logging-data-events-with-cloudtrail.md#creating-data-event-selectors-advanced)\. You can keep the event data in an event data store for up to seven years, or 2557 days\. By default, event data is retained for the maximum period, 2557 days\. The selectors that you apply to an event data store control which events persist and are available for you to query\. CloudTrail Lake is an auditing solution that can complement your compliance stack, and assist you with near real\-time troubleshooting\.

CloudTrail Lake queries offer a deeper and more customizable view of events than simple key and value lookups in **Event history**, or running `LookupEvents`\. An **Event history** search is limited to a single AWS account, only returns events from a single region, and cannot query multiple attributes\. By contrast, CloudTrail Lake users can run complex Standard Query Language \(SQL\) queries across multiple fields in a CloudTrail event\. CloudTrail Lake can aggregate information from your enterprise into a single, searchable event data store, and search across all regions at once\. For a full list of supported SQL operators, see [CloudTrail Lake SQL constraints](query-limitations.md)\.

You can save Lake queries for future use, and view results of queries for up to seven days\. When you run queries, you can save the query results to an Amazon S3 bucket\. CloudTrail Lake can also store events from an organization in AWS Organizations in an event data store, including events from multiple regions and accounts\.

Though CloudTrail does not support authorization based on tags for trails, you can control access to actions on event data stores by using authorization based on tags\. For more information and examples, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags) in this guide\.

By default, all events in an event data store are encrypted by CloudTrail\. When you configure an event data store, you can choose to use your own AWS Key Management Service key\. Using your own KMS key incurs AWS KMS costs for encryption and decryption\. After you associate an event data store with a KMS key, the KMS key cannot be removed or changed\.

CloudTrail Lake event data stores and queries incur CloudTrail charges\. For more information about CloudTrail Lake pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

CloudTrail Lake supports Amazon CloudWatch metrics, which you can use to view information about the amount of data ingested into your event data store during the last hour and over the course of its retention period\. For more information about supported CloudWatch metrics, see [Supported CloudWatch metrics](cloudtrail-lake-cloudwatch-metrics.md)\.

**Note**  
CloudTrail typically delivers logs within an average of about 15 minutes of an API call\. This time is not guaranteed\.

**Topics**
+ [Create an event data store](query-event-data-store.md)
+ [Manage event data store lifecycles](query-eds-disable-termination.md)
+ [Copy trail events to an event data store](cloudtrail-copy-trail-to-lake-eds.md)
+ [Create or edit a query](query-create-edit-query.md)
+ [Run a query and save query results](query-run-query.md)
+ [View query results](query-results.md)
+ [Get and download saved query results](view-download-cloudtrail-lake-query-results.md)
+ [Validate saved query results](cloudtrail-query-results-validation-intro.md)
+ [Viewing service\-linked channels for CloudTrail by using the AWS CLI](viewing-service-linked-channels.md)
+ [Managing CloudTrail Lake by using the AWS CLI](query-lake-cli.md)
+ [CloudTrail Lake SQL constraints](query-limitations.md)
+ [Example queries](query-lake-examples.md)
+ [Supported CloudWatch metrics](cloudtrail-lake-cloudwatch-metrics.md)