# Working with AWS CloudTrail Lake<a name="cloudtrail-lake"></a>

AWS CloudTrail Lake lets you run SQL\-based queries on your events\. Events are aggregated into event data stores, which are immutable collections of events based on criteria that you select by applying [advanced event selectors](logging-data-events-with-cloudtrail.md#creating-data-event-selectors-advanced)\. You can keep the event data in an event data store for up to seven years, or 2555 days\. By default, event data is retained for the maximum period, 2555 days\. The selectors that you apply to an event data store control which events persist and are available for you to query\. CloudTrail Lake is an auditing solution that can complement your compliance stack, and assist you with real\-time troubleshooting\.

CloudTrail Lake queries offer a deeper and more customizable view of events than simple key and value lookups in **Event history**, or running `LookupEvents`\. An **Event history** search is limited to a single AWS account, only returns events from a single region, and cannot query multiple attributes\. By contrast, CloudTrail Lake users can run complex Standard Query Language \(SQL\) queries across multiple fields in a CloudTrail event\. CloudTrail Lake can aggregate information from your enterprise into a single, searchable event data store, and search across all regions at once\. For a full list of supported SQL operators, see [CloudTrail Lake SQL constraints](query-limitations.md)\.

You can save Lake queries for future use, and view results of queries for up to seven days\. CloudTrail Lake can also store events from an organization in AWS Organizations in an event data store, including events from multiple regions and accounts\.

Though CloudTrail does not support authorization based on tags for trails, you can control access to actions on event data stores by using authorization based on tags\. For more information and examples, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags) in this guide\.

CloudTrail Lake event data stores and queries incur CloudTrail charges\. For more information about CloudTrail Lake pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.

**Topics**
+ [Create an event data store](query-event-data-store.md)
+ [Manage event data store lifecycles](query-eds-disable-termination.md)
+ [Create or edit a query](query-create-edit-query.md)
+ [Run a query](query-run-query.md)
+ [View query results](query-results.md)
+ [Managing CloudTrail Lake by using the AWS CLI](query-lake-cli.md)
+ [CloudTrail Lake SQL constraints](query-limitations.md)
+ [Example queries](query-lake-examples.md)