# CloudTrail Lake integrations event schema<a name="query-integration-event-schema"></a>

The following table describes the required and optional schema elements that match those in CloudTrail event records\. The contents of `eventData` are provided by your events; other fields are provided by CloudTrail after ingestion\.

CloudTrail event record contents are described in more detail in [CloudTrail record contents](cloudtrail-event-reference-record-contents.md)\.
+ [Fields that are provided by CloudTrail after ingestion](#fields-cloudtrail)
+ [Fields that are provided by your events](#fields-event)<a name="fields-cloudtrail"></a>


**Fields that are provided by CloudTrail after ingestion**  

| Field name | Input type | Requirement | Description | 
| --- | --- | --- | --- | 
| eventVersion | string | Required |  The event version\.  | 
| eventCategory | string | Required |  The event category\. For non\-AWS events, the value is `ActivityAuditLog`\.  | 
| eventType | string | Required |  The event type\. For non\-AWS events, the valid value is `ActivityLog`\.  | 
| eventID | string | Required | A unique ID for an event\. | 
| eventTime |  string  | Required |  Event timestamp, in `yyyy-MM-DDTHH:mm:ss` format, in Universal Coordinated Time \(UTC\)\.  | 
| awsRegion | string | Required |  The AWS Region where the `PutAuditEvents` call was made\.  | 
| recipientAccountId |  | Required |  Represents the account ID that received this event\. CloudTrail populates this field by calculating it from event payload\.  | 
| addendum |  \-  | Optional |  Shows information about why event processing was delayed\. If information was missing from an existing event, the addendum block includes the missing information and a reason for why it was missing\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Optional |  The reason that the event or some of its contents were missing\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Optional |  The event record fields that are updated by the addendum\. This is only provided if the reason is `UPDATED_DATA`\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Optional |  The original event UID from the source\. This is only provided if the reason is `UPDATED_DATA`\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Optional |  The original event ID\. This is only provided if the reason is `UPDATED_DATA`\.  | 
| metadata |  \-  | Required |  Information about the channel that the event used\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Required |  The timestamp when the event was processed, in `yyyy-MM-DDTHH:mm:ss` format, in Universal Coordinated Time \(UTC\)\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Required |  The ARN of the channel that the event used\.  | <a name="fields-event"></a>


**Fields that are provided by customer events**  

| Field name | Input type | Requirement | Description | 
| --- | --- | --- | --- | 
| eventData |  \-  | Required | The audit data sent to CloudTrail in a PutAuditEvents call\. | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Required |  The version of the event from its source\. Length constraints: Maximum length of 256\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  \-  | Required |  Information about the user who made a request\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Required |  The type of user identity\. Length constraints: Maximum length of 128\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Required |  A unique identifier for the actor of the event\. Length constraints: Maximum length of 1024\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  JSON object  | Optional |  Additional information about the identity\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Optional |  The agent through which the request was made\. Length constraints: Maximum length of 1024\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Required |  This is the partner event source, or the custom application about which events are logged\. Length constraints: Maximum length of 1024\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Required |  The requested action, one of the actions in the API for the source service or application\. Length constraints: Maximum length of 1024\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Required |  Event timestamp, in `yyyy-MM-DDTHH:mm:ss` format, in Universal Coordinated Time \(UTC\)\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Required |  The UID value that identifies the request\. The service or application that is called generates this value\. Length constraints: Maximum length of 1024\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  JSON object  | Optional |  The parameters, if any, that were sent with the request\. This field has a maximum size of 100 kB, and content exceeding the limit is rejected\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  JSON object  | Optional |  The response element for actions that make changes \(create, update, or delete actions\)\. This field has a maximum size of 100 kB, and content exceeding the limit is rejected\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Optional |  A string representing an error for the event\. Length constraints: Maximum length of 256\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Optional |  The description of the error\. Length constraints: Maximum length of 256\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  string  | Optional |  The IP address from which the request was made\. Both IPv4 and IPv6 addresses are accepted\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  | string | Required |  Represents the account ID that received this event\. The account ID must be the same as the AWS account ID that owns the channel\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-integration-event-schema.html)  |  JSON object  | Optional |  Additional data about the event that was not part of the request or response\. This field has a maximum size of 28 kB, and content exceeding that limit is rejected\.  | 

The following example shows the hierarchy of schema elements that match those in CloudTrail event records\.

```
{
    "eventVersion": String,
    "eventCategory": String,
    "eventType": String,
    "eventID": String,
    "eventTime": String,
    "awsRegion": String,
    "recipientAccountId": String,
    "addendum": {
       "reason": String,
       "updatedFields": String,
       "originalUID": String, 
       "originalEventID": String
    },
    "metadata" : { 
       "ingestionTime": String,
       "channelARN": String
    },
    "eventData": {
        "version": String,
        "userIdentity": {
          "type": String,
          "principalId": String,
          "details": {
             JSON
          }
        }, 
        "userAgent": String,
        "eventSource": String,
        "eventName": String,
        "eventTime": String,
        "UID": String,
        "requestParameters": {
           JSON
        },
        "responseElements": {
           JSON
        },
        "errorCode": String,
        "errorMessage": String,
        "sourceIPAddress": String,
        "recipientAccountId": String,
        "additionalEventData": {
           JSON
        }
    }
}
```