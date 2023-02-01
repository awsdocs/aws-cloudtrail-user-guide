# Configuration item schema<a name="query-event-data-store-config-schema"></a>

The following table describes the required and optional schema elements that match those in configuration item records\. The contents of `eventData` are provided by your configuration items; other fields are provided by CloudTrail after ingestion\.

CloudTrail event record contents are described in more detail in [CloudTrail record contents](cloudtrail-event-reference-record-contents.md)\.
+ [Fields that are provided by CloudTrail after ingestion](#fields-cloudtrail-event)
+ [Fields that are provided by your events](#fields-config)<a name="fields-cloudtrail-event"></a>


**Fields that are provided by CloudTrail after ingestion**  

| Field name | Input type | Requirement | Description | 
| --- | --- | --- | --- | 
| eventVersion | string | Required |  The version of the AWS event format\.  | 
| eventCategory | string | Required |  The event category\. For configuration items, the valid value is `ConfigurationItem`\.  | 
| eventType | string | Required |  The event type\. For configuration items, the valid value is `AwsConfigurationItem`\.  | 
| eventID | string | Required |  A unique ID for an event\.  | 
| eventTime |  string  | Required |  The event timestamp, in `yyyy-MM-DDTHH:mm:ss` format, in Universal Coordinated Time \(UTC\)\.  | 
| awsRegion | string | Required |  The AWS Region to which to assign an event\.  | 
| recipientAccountId | string | Required |  Represents the AWS account ID that received this event\.  | 
| addendum |  addendum  | Optional |  Shows information about why an event was delayed\. If information was missing from an existing event, the addendum block includes the missing information and a reason for why it was missing\.  | <a name="fields-config"></a>


**Fields in `eventData` are provided by your configuration items**  

| Field name | Input type | Requirement | Description | 
| --- | --- | --- | --- | 
| eventData |  \-  | Required | Fields in eventData are provided by your configuration items\. | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The version of the configuration item from its source\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The time when the configuration recording was initiated\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The configuration item status\. Valid values are `OK`, `ResourceDiscovered`, `ResourceNotRecorded`, ` ResourceDeleted`, and `ResourceDeletedNotRecorded`\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The 12\-digit AWS account ID associated with the resource\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The type of AWS resource\. For more information about valid resource types, see [ConfigurationItem](https://docs.aws.amazon.com/config/latest/APIReference/API_ConfigurationItem.html) in the *AWS Config API Reference*\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The ID of the resource \(for example\., sg\-*xxxxxx*\)\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  The custom name of the resource, if available\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | string | Optional |  Amazon Resource Name \(ARN\) associated with the resource\.   | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The AWS Region where the resource resides\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The Availability Zone associated with the resource\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The time stamp when the resource was created\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  JSON  | Optional |  The description of the resource configuration\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  JSON  | Optional |  Configuration attributes that AWS Config returns for certain resource types to supplement the information returned for the configuration parameter\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  A list of CloudTrail event IDs\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  | \- | Optional |  A list of related AWS resources\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The type of relationship with the related resource\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The resource type of the related resource\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The ID of the related resource \(for example, sg\-*xxxxxx*\)\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  string  | Optional |  The custom name of the related resource, if available\.  | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-config-schema.html)  |  JSON  | Optional |  A mapping of key value tags associated with the resource\.  | 

The following example shows the hierarchy of schema elements that match those in configuration item records\.

```
{
  "eventVersion": String,
  "eventCategory: String,
  "eventType": String,
  "eventID": String,
  "eventTime": String,
  "awsRegion": String,
  "recipientAccountId": String,
  "addendum": Addendum,
  "eventData": {
      "configurationItemVersion": String,
      "configurationItemCaptureTime": String,
      "configurationItemStatus": String,
      "configurationStateId": String,
      "accountId": String,
      "resourceType": String,
      "resourceId": String,
      "resourceName": String,
      "arn": String,
      "awsRegion": String, 
      "availabilityZone": String,
      "resourceCreationTime": String,
      "configuration": {
        JSON,
      },
      "supplementaryConfiguration": {
        JSON,
      },
      "relatedEvents": [
        String
      ],
      "relationships": [
        struct{
          "name" : String,
          "resourceType": String,
          "resourceId": String,
          "resourceName": String
        }
      ],
     "tags": {
       JSON
     }
    }
  }
}
```