# Create an event data store<a name="query-event-data-store"></a>

When you create an event data store in CloudTrail Lake, you choose the category of AWS events to include in your event data store\. You can create an event data store to include CloudTrail events, or AWS Config configuration items\. Each event data store can only contain a single event category \(for example, AWS Config configuration items\), because the event schema for each event category is unique\. You can run SQL queries across multiple event data stores using the supported SQL JOIN keywords\. For information about running queries across multiple event data stores, see [Advanced, multi\-table query support](query-limitations.md#query-advanced-multi-table)\.

The sections which follow describe how to create an event data store for each event category\.

**Topics**
+ [Create an event data store for CloudTrail events](query-event-data-store-cloudtrail.md)
+ [Create an event data store for AWS Config configuration items](query-event-data-store-config.md)