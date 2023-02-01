# Managing CloudTrail Lake by using the AWS CLI<a name="query-lake-cli"></a>

The following are example AWS CLI commands for creating and managing event data stores and queries in CloudTrail Lake\.

**Topics**
+ [Create an event data store for CloudTrail data events](#lake-cli-create-eds)
+ [Create an event data store for AWS Config configuration items](#lake-cli-create-eds-config)
+ [Create an integration to log events from outside AWS](#lake-cli-create-integration)
+ [Get an event data store](#lake-cli-get-eds)
+ [List all event data stores in an account](#lake-cli-list-eds)
+ [Update an event data store](#lake-cli-update-eds)
+ [Delete an event data store](#lake-cli-delete-eds)
+ [Restore an event data store](#lake-cli-restore-eds)
+ [List all channels](#lake-cli-list-channels)
+ [Update a channel](#lake-cli-update-channel)
+ [Delete a channel to delete an integration](#lake-cli-delete-integration)
+ [Start a query](#lake-cli-start-query)
+ [Get metadata about a query](#lake-cli-describe-query)
+ [Get query results](#lake-cli-get-query-results)
+ [List all queries on an event data store](#lake-cli-list-queries)
+ [Cancel a running query](#lake-cli-cancel-query)

## Create an event data store for CloudTrail data events<a name="lake-cli-create-eds"></a>

The following example AWS Command Line Interface \(AWS CLI\) command creates an event data store named `my-event-data-store` that selects all Amazon S3 data events\. The event data store retention period in this example is 90 days\.`--name` is required; other parameters are optional\. Valid values for `--retention-period` are integers between 7 and 2557, representing days\. If you do not specify `--retention-period`, CloudTrail uses the default retention period of 2557 days\. By default, `--multi-region-enabled` is set, even if the parameter is not added, and the event data store includes events from all Regions\. The event data store is not enabled for all accounts in an AWS Organizations organization by default\. To enable an event data store to collect events for all accounts in an organization, add the `--organization-enabled` parameter\. The parameters `--termination-protection-enabled` \(the default\) and `--no-termination-protection-enabled` set and remove termination protection, respectively\. Optionally, you can choose to enable AWS Key Management Service encryption and specify an AWS KMS key by adding `--kms-key-id` to the command, and specifying a KMS key ARN as the value\. The `--advanced-event-selectors` parameter includes or excludes data events in your event data store\. For more information about `--advanced-event-selectors`, see [https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutEventSelectors.html#awscloudtrail-PutEventSelectors-request-AdvancedEventSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutEventSelectors.html#awscloudtrail-PutEventSelectors-request-AdvancedEventSelectors) in the CloudTrail API Reference\.

```
aws cloudtrail create-event-data-store \
--name my-event-data-store \
--retention-period 90 \
--kms-key-id "arn:aws:kms:us-east-1:0123456789:alias/KMS_key_alias" \
--advanced-event-selectors '[
        {
            "Name": "Select all S3 data events",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
                { "Field": "resources.ARN", "StartsWith": ["arn:aws:s3"] }
            ]
        }
    ]'
```

The following is an example response\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "CREATED",
    "AdvancedEventSelectors": [
        {
            "Name": "Select all S3 data events",
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
                        "arn:aws:s3"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 90,
    "KmsKeyId": "arn:aws:kms:us-east-1:0123456789:alias/KMS_key_alias",
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2021-12-09T22:19:39.417000-05:00",
    "UpdatedTimestamp": "2021-12-09T22:19:39.603000-05:00"
}
```

## Create an event data store for AWS Config configuration items<a name="lake-cli-create-eds-config"></a>

The following example AWS CLI create\-event\-data\-store command creates an event data store named `my-event-data-store` that selects AWS Config configuration items\. The event data store retention period in this example is 90 days\. The `--name` parameter is required; other parameters are optional\. Valid values for `--retention-period` are integers between 7 and 2557, representing days\. If you don't specify `--retention-period`, CloudTrail uses the default retention period of 2557 days\. By default, `--multi-region-enabled` is set, even if the parameter is not added, and the event data store includes configuration items from all AWS Regions\. The event data store is not enabled for all accounts in an AWS Organizations organization by default; to enable an event data store to collect configuration items for all accounts in an organization, add the `--organization-enabled` parameter\. The parameters `--termination-protection-enabled` \(the default\) and `--no-termination-protection-enabled` set and remove termination protection, respectively\. Optionally, you can choose to enable AWS Key Management Service encryption and specify an AWS KMS key by adding `--kms-key-id` to the command, and specifying a KMS key ARN as the value\. The `--advanced-event-selectors` parameter includes configuration items in your event data store by specifying the `eventCategory` field equals `ConfigurationItem`\.

```
aws cloudtrail create-event-data-store
--name my-event-data-store
--retention-period 90
--kms-key-id "arn:aws:kms:us-east-1:0123456789:alias/KMS_key_alias" \
--advanced-event-selectors '[
        {
            "Name": "Select AWS Config configuration items",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["ConfigurationItem"] }
            ]
        }
    ]'
```

The following is an example response\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "CREATED",
    "AdvancedEventSelectors": [
        {
            "Name": "Select AWS Config configuration items",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "ConfigurationItem"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 90,
    "KmsKeyId": "arn:aws:kms:us-east-1:0123456789:alias/KMS_key_alias",
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2022-10-07T19:03:24.277000+00:00",
    "UpdatedTimestamp": "2022-10-07T19:03:24.468000+00:00"
}
```

## Create an integration to log events from outside AWS<a name="lake-cli-create-integration"></a>

**Note**  
Currently, integrations are supported in all commercial AWS Regions supported by CloudTrail Lake except: me\-central\-1\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

In the AWS CLI, you create an integration that logs events from outside AWS in four commands \(three if you already have an event data store that meets the criteria\)\. Event data stores that you use as the destinations for an integration must be for a single Region and single account; they cannot be multi\-region, they cannot log events for organizations in AWS Organizations, and they can only include activity events\. The event type in the console must be **Events from integrations**\. In the API, the `eventCategory` value must be `ActivityAuditLog`\. For more information about integrations, see [Create an integration with an event source outside of AWS](query-event-data-store-integration.md)\.

1. Run [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/index.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/index.html) to create an event data store, if you do not already have one or more event data stores that you can use for the integration\.

   The following example AWS CLI command creates an event data store that logs events from outside AWS\. For activity events, the `eventCategory` field selector value is `ActivityAuditLog`\. The event data store has a retention period of 90 days set\. By default, the event data store collects events from all Regions, but because this is collecting non\-AWS events, set it to a single Region by adding the `--no-multi-region-enabled` option\. Termination protection is enabled by default, and the event data store does not collect events for accounts in an organization\.

   ```
   aws cloudtrail create-event-data-store \
   --name my-event-data-store \
   --no-multi-region-enabled \
   --retention-period 90 \
   --advanced-event-selectors '[
       {
         "Name": "Select all external events",
         "FieldSelectors": [
             { "Field": "eventCategory", "Equals": ["ActivityAuditLog"] }
           ]
       }
     ]'
   ```

   The following is an example response\.

   ```
   {
       "Name": "my-event-data-store",
       "ARN": "arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLEf852-4e8f-8bd1-bcf6cEXAMPLE",
       "RetentionPeriod": "90",
       "MultiRegionEnabled": false,
       "OrganizationEnabled": false,
       "TerminationProtectionEnabled": true,
       "AdvancedEventSelectors": [
           {
              "Name": "Select all external events",
              "FieldSelectors": [
                 {
                     "Field": "eventCategory",
                     "Equals": [
                         "ActivityAuditLog"
                       ]
                   }
               ]
           }
       ]
   }
   ```

   You'll need the event data store ID \(the suffix of the ARN, or `EXAMPLEf852-4e8f-8bd1-bcf6cEXAMPLE` in the preceding response example\) to go on to the next step and create your channel\.

1. Run the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/create-channel.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/create-channel.html) command to create a channel that allows a partner or source application to send events to an event data store in CloudTrail\.

   A channel has the following components:  
**Source**  
CloudTrail uses this information to determine the partners that are sending event data to CloudTrail on your behalf\. A source is required, and can be either `Custom` for all valid non\-AWS events, or the name of a partner event source\. A maximum of one channel is allowed per source\.  
For information about the `Source` values for available partners, see [Additional information about integration partners](query-event-data-store-integration.md#cloudtrail-lake-partner-information)\.  
**Ingestion status**  
The channel status shows when the last events were received from a channel source\.  
**Destinations**  
The destinations are the CloudTrail Lake event data stores that are receiving events from the channel\. You can change destination event data stores for a channel\.

   To stop receiving events from a source, delete the channel\.

   You need the ID of at least one destination event data store to run this command\. The valid type of destination is `EVENT_DATA_STORE`\. You can send ingested events to more than one event data store\. The following example command creates a channel that sends events to two event data stores, represented by their IDs in the `Location` attribute of the `--destinations` parameter\. The `--destinations`, `--name`, and `--source` parameters are required\. To ingest events from a CloudTrail partner, specify the name of the partner as the value of `--source`\. To ingest events from your own applications outside AWS, specify `Custom` as the value of `--source`\.

   ```
   aws cloudtrail create-channel \
       --region us-east-1 \
       --destinations '[{"Type": "EVENT_DATA_STORE", "Location": "EXAMPLEf852-4e8f-8bd1-bcf6cEXAMPLE"}, {"Type": "EVENT_DATA_STORE", "Location": "EXAMPLEg922-5n2l-3vz1- apqw8EXAMPLE"}]'
       --name my-partner-channel \
       --source $partnerSourceName \
   ```

   In the response to your create\-channel command, copy the ARN of the new channel\. You need the ARN to run the `put-resource-policy` and `put-audit-events` commands in the next steps\.

1.  Run the **put\-resource\-policy** command to attach a resource policy to the channel\. Resource policies are JSON policy documents that specify what actions a specified principal can perform on the resource and under what conditions\. The accounts defined as principals in the channel's resource policy can call the `PutAuditEvents` API to deliver events\. 
**Note**  
If you do not create a resource policy for the channel, only the channel owner can call the `PutAuditEvents` API on the channel\.

   The information required for the policy is determined by the integration type\.
   + For a direction integration, CloudTrail requires the policy to contain the partner's AWS account IDs, and requires you to enter the unique external ID provided by the partner\. CloudTrail automatically adds the partner's AWS account IDs to the resource policy when you create an integration using the CloudTrail console\. Refer to the [partner's documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-integration.html#cloudtrail-lake-partner-information#lake-integration-partner-documentation) to learn how to get the AWS account numbers required for the policy\.
   + For a solution integration, you must specify at least one AWS account ID as principal, and can optionally enter an external ID to prevent against confused deputy\.

   The following are requirements for the resource policy:
   + The resource ARN defined in the policy must match the channel ARN the policy is attached to\.
   +  The policy contains only one action: cloudtrail\-data:PutAuditEvents 
   +  The policy contains at least one statement\. The policy can have a maximum of 20 statements\. 
   +  Each statement contains at least one principal\. A statement can have a maximum of 50 principals\. 

   ```
   aws cloudtrail put-resource-policy \
       --resource-arn "channelARN" \
       --policy "{
       "Version": "2012-10-17",
       "Statement":
       [
           {
               "Sid": "ChannelPolicy",
               "Effect": "Allow",
               "Principal":
               {
                   "AWS":
                   [
                       "arn:aws:iam::111122223333:root",
                       "arn:aws:iam::444455556666:root",
                       "arn:aws:iam::123456789012:root"
                   ]
               },
               "Action": "cloudtrail-data:PutAuditEvents",
               "Resource": "arn:aws:cloudtrail:us-east-1:777788889999:channel/EXAMPLE-80b5-40a7-ae65-6e099392355b",
               "Condition":
               {
                   "StringEquals":
                   {
                       "cloudtrail:ExternalId": "UniqueExternalIDFromPartner"
                   }
               }
           }
       ]
   }"
   ```

   For more information about resource policies, see [AWS CloudTrail resource\-based policy examples](security_iam_resource-based-policy-examples.md)\.

1. Run the [https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html](https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html) API to ingest your activity events into CloudTrail\. You'll need the payload of events that you want CloudTrail to add\. Be sure that there is no sensitive or personally\-identifying information in event payload before ingesting it into CloudTrail\. Note that the `PutAuditEvents` API uses the `cloudtrail-data` CLI endpoint, not the `cloudtrail` endpoint\.

   The following examples show how to use the put\-audit\-events CLI command\. The \-\-audit\-events and \-\-channel\-arn parameters are required\. The \-\-external\-id parameter is required if an external ID is defined in the resource policy\. You need the ARN of the channel that you created in the preceding step\. The value of \-\-audit\-events is a JSON array of event objects\. `--audit-events` includes a required ID from the event, the required payload of the event as the value of `EventData`, and an [optional checksum](#lake-cli-integration-checksum.title) to help validate the integrity of the event after ingestion into CloudTrail\.

   ```
   aws cloudtrail-data put-audit-events \
   --channel-arn $ChannelArn \
   --external-id $UniqueExternalIDFromPartner \
   --audit-events \
   id="event_ID",eventData='"{event_payload}"' \
   id="event_ID",eventData='"{event_payload}"',eventDataChecksum="optional_checksum"
   ```

   The following is an example command with two event examples\.

   ```
   aws cloudtrail-data put-audit-events \
   --channel-arn arn:aws:cloudtrail:us-east-1:01234567890:channel/EXAMPLE8-0558-4f7e-a06a-43969EXAMPLE \
   --external-id UniqueExternalIDFromPartner \
   --audit-events \
   id="EXAMPLE3-0f1f-4a85-9664-d50a3EXAMPLE",eventData='"{\"eventVersion\":\0.01\",\"eventSource\":\"custom1.domain.com\", ...
   \}"' \
   id="EXAMPLE7-a999-486d-b241-b33a1EXAMPLE",eventData='"{\"eventVersion\":\0.02\",\"eventSource\":\"custom2.domain.com\", ...
   \}"',eventDataChecksum="EXAMPLE6e7dd61f3ead...93a691d8EXAMPLE"
   ```

   The following example command adds the `--cli-input-json` parameter to specify a JSON file \(`custom-events.json`\) of event payload\.

   ```
   aws cloudtrail-data put-audit-events --channel-arn $channelArn --external-id $UniqueExternalIDFromPartner --cli-input-json file://custom-events.json --region us-east-1
   ```

   The following are the sample contents of the example JSON file, `custom-events.json`\.

   ```
   {
       "auditEvents": [
         {
           "eventData": "{\"version\":\"eventData.version\",\"UID\":\"UID\",
           \"userIdentity\":{\"type\":\"CustomUserIdentity\",\"principalId\":\"principalId\",
           \"details\":{\"key\":\"value\"}},\"eventTime\":\"2021-10-27T12:13:14Z\",\"eventName\":\"eventName\",
           \"userAgent\":\"userAgent\",\"eventSource\":\"eventSource\",
           \"requestParameters\":{\"key\":\"value\"},\"responseElements\":{\"key\":\"value\"},
           \"additionalEventData\":{\"key\":\"value\"},
           \"sourceIPAddress\":\"12.34.56.78\",\"recipientAccountId\":\"152089810396\"}",
           "id": "1"
         }
      ]
   }
   ```

You can verify that the integration is working, and CloudTrail is ingesting events from the source correctly, by running the [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/get-channel.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/get-channel.html) command\. The output of get\-channel shows the most recent time stamp that CloudTrail received events\.

```
aws cloudtrail get-channel --channel arn:aws:cloudtrail:us-east-1:01234567890:channel/EXAMPLE8-0558-4f7e-a06a-43969EXAMPLE
```

### \(Optional\) Calculate a checksum value<a name="lake-cli-integration-checksum"></a>

The checksum that you specify as the value of `EventDataChecksum` in a `PutAuditEvents` request helps you verify that CloudTrail receives the event that matches with the checksum; it helps verify the integrity of events\. The checksum value is a base64\-SHA256 algorithm that you calculate by running the following command\.

```
printf %s "{"eventData": "{\"version\":\"eventData.version\",\"UID\":\"UID\",
        \"userIdentity\":{\"type\":\"CustomUserIdentity\",\"principalId\":\"principalId\",
        \"details\":{\"key\":\"value\"}},\"eventTime\":\"2021-10-27T12:13:14Z\",\"eventName\":\"eventName\",
        \"userAgent\":\"userAgent\",\"eventSource\":\"eventSource\",
        \"requestParameters\":{\"key\":\"value\"},\"responseElements\":{\"key\":\"value\"},
        \"additionalEventData\":{\"key\":\"value\"},
        \"sourceIPAddress\":\"source_IP_address\",
        \"recipientAccountId\":\"recipient_account_ID\"}",
        "id": "1"}" \
 | openssl dgst -binary -sha256 | base64
```

The command returns the checksum\. The following is an example\.

```
EXAMPLEDHjkI8iehvCUCWTIAbNYkOgO/t0YNw+7rrQE=
```

The checksum value becomes the value of `EventDataChecksum` in your `PutAuditEvents` request\. If the checksum doesn't match with the one for the provided event, CloudTrail rejects the event with an `InvalidChecksum` error\.

## Get an event data store<a name="lake-cli-get-eds"></a>

The following example AWS CLI get\-event\-data\-store command returns information about the event data store specified by the required `--event-data-store` parameter, which accepts an ARN or the ID suffix of the ARN\.

```
aws cloudtrail get-event-data-store
--event-data-store arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
```

The following is an example response\. Creation and last updated times are in `timestamp` format\.

```
{
    "EventDataStoreARN": "arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "Enabled",
    "KmsKeyId": "kms_key_ID",
    "AdvancedEventSelectors": [
        {
            "Name": "Select All S3 Data Events",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
                { "Field": "resources.ARN", "StartsWith": ["arn:aws:s3"] }
            ]
        }
    ]
    "CreatedTimestamp": 1248496624,
    "UpdatedTimestamp": 1598296624,
    "MultiRegionEnabled": true,
    "RetentionPeriod": 90,
    "TerminationProtectionEnabled": true,
}
```

## List all event data stores in an account<a name="lake-cli-list-eds"></a>

The following example AWS CLI list\-event\-data\-stores command returns information about all event data stores in an account, in the current Region\. Optional parameters include `--max-results`, to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail list-event-data-stores
```

The following is an example response\.

```
{
    "EventDataStores": [
        {
            "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
            "Name": "my-event-data-store"
        }
    ]
}
```

## Update an event data store<a name="lake-cli-update-eds"></a>

The following example AWS CLI update\-event\-data\-store command updates an event data store to change its retention period to 100 days, and enable termination protection\. The required `--event-data-store` parameter value is an ARN \(or the ID suffix of the ARN\) and is required; other parameters are optional\. In this example, the `--retention-period` parameter is added to change the retention period to 100 days\. Optionally, you can choose to enable AWS Key Management Service encryption and specify an AWS KMS key by adding `--kms-key-id` to the command, and specifying a KMS key ARN as the value\. `--termination-protection-enabled` is added to enable termination protection on an event data store that did not have termination protection enabled\.

An event data store that logs events from outside AWS cannot be updated to log AWS events\. Similarly, an event data store that logs AWS events cannot be updated to log events from outside AWS\.AWS

```
aws cloudtrail update-event-data-store
--event-data-store arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--retention-period 100
--kms-key-id "arn:aws:kms:us-east-1:0123456789:alias/KMS_key_alias" \
--termination-protection-enabled
```

The following is an example response\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "ENABLED",
    "KmsKeyId": "kms_key_ID",
    "AdvancedEventSelectors": [
        {
            "Name": "Select all S3 data events",
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
                        "arn:aws:s3"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 100,
    "KmsKeyId": "arn:aws:kms:us-east-1:0123456789:alias/KMS_key_alias"
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2021-12-09T22:19:39.417000-05:00",
    "UpdatedTimestamp": "2021-12-09T22:19:39.603000-05:00"
}
```

## Delete an event data store<a name="lake-cli-delete-eds"></a>

The following example AWS CLI delete\-event\-data\-store command disables the event data store specified by `--event-data-store`, which accepts an event data store ARN, or the ID suffix of the ARN\. After you run delete\-event\-data\-store, the final state of the event data store is `PENDING_DELETION`, and the event data store is automatically deleted after a wait period of seven days\. `--no-termination-protection-enabled` must be set on the event data store; this operation cannot work if `--termination-protection-enabled` is set\.

After you run delete\-event\-data\-store on an event data store, you cannot run list\-queries, describe\-query, or get\-query\-results on queries that are using the disabled data store\. The event data store does not count towards your account maximum of five event data stores when it is pending deletion\.

```
aws cloudtrail delete-event-data-store
--event-data-store arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
```

There is no response if the operation is successful\.

## Restore an event data store<a name="lake-cli-restore-eds"></a>

The following example AWS CLI restore\-event\-data\-store command restores an event data store that is pending deletion\. The event data store is specified by `--event-data-store`, which accepts an event data store ARN or the ID suffix of the ARN\. You can only restore a deleted event data store within the seven\-day wait period after deletion\.

```
aws cloudtrail restore-event-data-store
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
```

The response includes information about the event data store, including its ARN, advanced event selectors, and the status of restoration\.

## List all channels<a name="lake-cli-list-channels"></a>

To list all channels in your account, run the list\-channels command\. The following is an example\.

```
aws cloudtrail list-channels
```

## Update a channel<a name="lake-cli-update-channel"></a>

To update a channel's name or destination event data stores, run the update\-channel command\. The `--channel` parameter is required\. You cannot update the source of a channel\. The following is an example\.

```
aws cloudtrail update-channel \
--channel aws:cloudtrail:us-east-1:01234567890:channel/EXAMPLE8-0558-4f7e-a06a-43969EXAMPLE \
--name "new-channel-name" \
--destinations '[{"Type": "EVENT_DATA_STORE", "Location": "EXAMPLEf852-4e8f-8bd1-bcf6cEXAMPLE"}, {"Type": "EVENT_DATA_STORE", "Location": "EXAMPLEg922-5n2l-3vz1- apqw8EXAMPLE"}]'
```

## Delete a channel to delete an integration<a name="lake-cli-delete-integration"></a>

To stop ingesting partner or other activity events outside AWS, delete the channel by running the delete\-channel command\. The ARN or channel ID \(the ARN suffix\) of the channel that you want to delete is required\. The following is an example\.

```
aws cloudtrail delete-channel \
--channel EXAMPLE8-0558-4f7e-a06a-43969EXAMPLE
```

## Start a query<a name="lake-cli-start-query"></a>

The following example AWS CLI start\-query command runs a query on the event data store specified as an ID in the query statement and delivers the query results to a specified S3 bucket\. The required `--query-statement` parameter provides a SQL query, enclosed in single quotation marks\. Optional parameters include `--delivery-s3uri`, to deliver the query results to a specified S3 bucket\. For more information about the query language you can use in CloudTrail Lake, see [CloudTrail Lake SQL constraints](query-limitations.md)\.

```
aws cloudtrail start-query
--query-statement 'SELECT eventID, eventTime FROM EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE LIMIT 10'
--delivery-s3uri "s3://aws-cloudtrail-lake-query-results-12345678910-us-east-1"
```

The response is a `QueryId` string\. To get the status of a query, run describe\-query using the `QueryId` value returned by start\-query\. If the query is successful, you can run get\-query\-results to get results\.

**Output**

```
{
    "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE"
}
```

**Note**  
Queries that run for longer than one hour might time out\. You can still get partial results that were processed before the query timed out\.  
If you are delivering the query results to an S3 bucket using the optional `--delivery-s3uri` parameter, the bucket policy must grant CloudTrail permission to delivery query results to the bucket\. For information about manually editing the bucket policy, see [Amazon S3 bucket policy for CloudTrail Lake query results](s3-bucket-policy-lake-query-results.md)\.

## Get metadata about a query<a name="lake-cli-describe-query"></a>

The following example AWS CLI describe\-query command gets metadata about a query, including query run time in milliseconds, number of events scanned and matched, total number of bytes scanned, and query status\. The `BytesScanned` value matches the number of bytes for which your account is billed for the query, unless the query is still running\.

You must specify a value for `--query-id`\.

```
aws cloudtrail describe-query
--query-id EXAMPLEd-17a7-47c3-a9a1-eccf7EXAMPLE
```

The following is an example response\.

```
{
    "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE", 
    "QueryString": "SELECT eventID, eventTime FROM EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE LIMIT 10", 
    "QueryStatus": "RUNNING",
    "QueryStatistics": {
        "EventsMatched": 10,
        "EventsScanned": 1000,
        "BytesScanned": 35059,
        "ExecutionTimeInMillis": 3821,
        "CreationTime": "1598911142"
    }
}
```

## Get query results<a name="lake-cli-get-query-results"></a>

The following example AWS CLI get\-query\-results command gets event data results of a query\. You must specify an ARN or the ID suffix of the ARN for `--event-data-store`, and the `QueryID` returned by the start\-query command\. The `BytesScanned` value matches the number of bytes for which your account is billed for the query, unless the query is still running\. Optional parameters include `--max-query-results`, to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-query-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail get-query-results
--query-id EXAMPLEd-17a7-47c3-a9a1-eccf7EXAMPLE
```

**Output**

```
{
    "QueryStatus": "RUNNING",
    "QueryStatistics": {
        "ResultsCount": 244,
        "TotalResultsCount": 1582,
        "BytesScanned":27044
    },
    "QueryResults": [
      {
        "key": "eventName",
        "value": "StartQuery",
      }
   ],
    "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE", 
    "QueryString": "SELECT eventID, eventTime FROM EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
 LIMIT 10",
    "NextToken": "20add42078135EXAMPLE"
}
```

## List all queries on an event data store<a name="lake-cli-list-queries"></a>

The following example AWS CLI list\-queries command returns a list of queries and query statuses on a specified event data store for the past seven days\. You must specify an ARN or the ID suffix of an ARN value for `--event-data-store`\. Optionally, to shorten the list of results, you can specify a time range, formatted as timestamps, by adding `--start-time` and `--end-time` parameters, and a `--query-status` value\. Valid values for `QueryStatus` include `QUEUED`, `RUNNING`, `FINISHED`, `FAILED`, or `CANCELLED`\.

list\-queries also has optional pagination parameters\. Use `--max-results` to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail list-queries
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--query-status CANCELLED
--start-time 1598384589
--end-time 1598384602
--max-results 10
```

**Output**

```
{
    "Queries": [
        {
          "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE", 
          "QueryStatus": "CANCELLED",
          "CreationTime": 1598911142
        },
        {
          "QueryId": "EXAMPLE2-4e89-9230-2127-5dr3aEXAMPLE", 
          "QueryStatus": "CANCELLED",
          "CreationTime": 1598296624"
        }
     ],
    "NextToken": "20add42078135EXAMPLE"
}
```

## Cancel a running query<a name="lake-cli-cancel-query"></a>

The following example AWS CLI cancel\-query command cancels a query with a status of `RUNNING`\. You must specify an ARN or the ID suffix of an ARN value for `--event-data-store`, and a value for `--query-id`\. When you run cancel\-query, the query status might show as `CANCELLED` even if the cancel\-query operation is not yet finished\.

**Note**  
A canceled query can incur charges\. Your account is still charged for the amount of data that was scanned before you canceled the query\.

The following is a CLI example\.

```
aws cloudtrail cancel-query
--query-id EXAMPLEd-17a7-47c3-a9a1-eccf7EXAMPLE
```

**Output**

```
QueryId -> (string)
QueryStatus -> (string)
```