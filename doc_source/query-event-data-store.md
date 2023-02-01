# Create an event data store<a name="query-event-data-store"></a>

When you create an event data store in CloudTrail Lake, you choose the type of events to include in your event data store\. You can create an event data store to include CloudTrail events, AWS Config configuration items, or events outside of AWS\. Each event data store can only contain a specific event category \(for example, AWS Config configuration items\), because the event schema is unique to the event category\. You can run SQL queries across multiple event data stores using the supported SQL JOIN keywords\. For information about running queries across multiple event data stores, see [Advanced, multi\-table query support](query-limitations.md#query-advanced-multi-table)\.

You can also create an event data store for AWS Audit Manager evidence by using the Audit Manager console\. For more information about aggregating evidence in CloudTrail Lake using Audit Manager, see [Understanding how evidence finder works with CloudTrail Lake](https://docs.aws.amazon.com/audit-manager/latest/userguide/evidence-finder.html#understanding-evidence-finder) in the *AWS Audit Manager User Guide*\.

The sections which follow describe how to create an event data store using the CloudTrail console\.

**Topics**
+ [Create an event data store for CloudTrail events](query-event-data-store-cloudtrail.md)
+ [Create an event data store for AWS Config configuration items](query-event-data-store-config.md)
+ [Create an event data store for events outside of AWS](event-data-store-integration-events.md)