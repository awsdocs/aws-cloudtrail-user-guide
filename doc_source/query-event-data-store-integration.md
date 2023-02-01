# Create an integration with an event source outside of AWS<a name="query-event-data-store-integration"></a>

**Note**  
Currently, integrations are supported in all commercial AWS Regions supported by CloudTrail Lake except: me\-central\-1\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

You can use CloudTrail to log and store user activity data from any source in your hybrid environments, such as in\-house or SaaS applications hosted on\-premises or in the cloud, virtual machines, or containers\. You can store, access, analyze, troubleshoot and take action on this data without maintaining multiple log aggregators and reporting tools\. 

Activity events from non\-AWS sources work by using *channels* to bring events into CloudTrail Lake from external partners that work with CloudTrail, or from your own sources\. When you create a channel, you choose one or more event data stores to store events that arrive from the channel source\. You can change the destination event data stores for a channel as needed, as long as the destination event data stores are set to log `eventCategory="ActivityAuditLog"` events\. When you create a channel for events from an external partner, you provide a channel ARN to the partner or source application\. The resource policy attached to the channel allows the source to transmit events through the channel\. If a channel does not have a resource policy, only the channel owner can call the `PutAuditEvents` API on the channel\.

CloudTrail has partnered with many event source providers, such as Okta and LaunchDarkly\. When you create an integration with an event source outside AWS, you can choose one of these partners as your event source, or choose **My custom integration** to integrate events from your own sources into CloudTrail\. A maximum of one channel is allowed per source\.

There are two types of integrations: direct and solution\. With direct integrations, the partner calls the `PutAuditEvents` API to deliver events to the event data store for your AWS account\. With solution integrations, the application runs in your AWS account and the application calls the `PutAuditEvents` API to deliver events to the event data store for your AWS account\.

From the **Integrations** page, you can choose the **Available sources** tab to the view the **Integration type** for partners\.

![\[Partner integration type\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/partner-integration-type.png)

To get started, create an integration to log events from partner or other application sources using the CloudTrail console\.

**Topics**
+ [Create an integration with a CloudTrail partner](#query-event-data-store-integration-partner)
+ [Create a custom integration](#query-event-data-store-integration-custom)
+ [Additional information about integration partners](#cloudtrail-lake-partner-information)
+ [CloudTrail Lake integrations event schema](query-integration-event-schema.md)

## Create an integration with a CloudTrail partner<a name="query-event-data-store-integration-partner"></a>

**Note**  
Currently, integrations are supported in all commercial AWS Regions supported by CloudTrail Lake except: me\-central\-1\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

When you create an integration with an event source outside AWS, you can choose one of these partners as your event source\. When you create an integration in CloudTrail with a partner application, the partner needs the Amazon Resource Name \(ARN\) of the channel that you create in this workflow to send events to CloudTrail\. After you create the integration, you finish configuring the integration by following the partner's instructions to provide the required channel ARN to the partner\. The integration starts ingesting partner events into CloudTrail after the partner calls `PutAuditEvents` on the integration's channel\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1.  From the navigation pane, open the **Lake** submenu, then choose **Integrations**\. 

1. On the **Add integration** page, enter a name for your channel\. The name can be 3\-128 characters\. Only letters, numbers, periods, underscores, and dashes are allowed\.

1. Choose the partner application source from which you want to get events\. If you're integrating with events from your own applications hosted on\-premises or in the cloud, choose **My custom integration**\.

1. From **Event delivery location**, choose to log the same activity events to existing event data stores, or create a new event data store\.

   If you choose to create a new event data store, enter a name for the event data store and specify the retention period in days\. Valid values for the retention period are integers from 7 to 2557 \(seven years\)\. The event data store retains event data for the specified number of days\. By default, event data is retained for 2557 days\.

   If you choose to log activity events to one or more existing event data stores, choose the event data stores from the list\. The event data stores can only include activity events\. The event type in the console must be **Events from integrations**\. In the API, the `eventCategory` value must be `ActivityAuditLog`\.

1. In **Resource policy**, configure the resource policy for the integration's channel\. Resource policies are JSON policy documents that specify what actions a specified principal can perform on the resource and under what conditions\. The accounts defined as principals in the resource policy can call the `PutAuditEvents` API to deliver events to your channel\. The resource owner has implicit access to the resource if their IAM policy allows the `cloudtrail-data:PutAuditEvents` action\.

   The information required for the policy is determined by the integration type\. For a direction integration, CloudTrail automatically adds the partner's AWS account IDs, and requires you to enter the unique external ID provided by the partner\. For a solution integration, you must specify at least one AWS account ID as principal, and can optionally enter an external ID to prevent against confused deputy\.
**Note**  
If you do not create a resource policy for the channel, only the channel owner can call the `PutAuditEvents` API on the channel\.

   1. For a direct integration, enter the external ID provided by your partner\. The integration partner provides a unique external ID, such as an account ID or a randomly generated string, to use for the integration to prevent against confused deputy\. The partner is responsible for creating and providing a unique external ID\.

       You can choose **How to find this?** to view the partner's documentation that describes how to find the external ID\.   
![\[Partner documentation for external ID\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/integration-external-id.png)
**Note**  
If the resource policy includes an external ID, all calls to the `PutAuditEvents` API must include the external ID\. However, if the policy does not define an external ID, the partner can still call the `PutAuditEvents` API and specify an `externalId` parameter\.

   1.  For a solution integration, choose **Add AWS account** to specify an AWS account ID to add as a principal in the policy\.

1. \(Optional\) In the **Tags** area, you can add up to 50 tag key and value pairs to help you identify, sort, and control access to your event data store and channel\. For more information about how to use IAM policies to authorize access to an event data store based on tags, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags)\. For more information about how you can use tags in AWS, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.

1. When you are ready to create the new integration, choose **Add integration**\. There is no review page\. CloudTrail creates the integration, but you must provide the channel Amazon Resource Name \(ARN\) to the partner application\. Instructions for providing the channel ARN to the partner application are found on the partner documentation website\. For more information, choose the **Learn more** link for the partner on the **Available sources** tab of the **Integrations** page to open the partner's page in AWS Marketplace\.

To finish the setup for your integration, provide the channel ARN to the partner or source application\. Depending upon the integration type, either you, the partner, or the application runs the `PutAuditEvents` API to deliver activity events to the event data store for your AWS account\. After your activity events are delivered, you can use CloudTrail Lake to search, query, and analyze the data that is logged from your applications\. Your event data includes fields that match CloudTrail event payload, such as `eventVersion`, `eventSource`, and `userIdentity`\.

## Create a custom integration<a name="query-event-data-store-integration-custom"></a>

**Note**  
Currently, integrations are supported in all commercial AWS Regions supported by CloudTrail Lake except: me\-central\-1\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\.

You can use CloudTrail to log and store user activity data from any source in your hybrid environments, such as in\-house or SaaS applications hosted on\-premises or in the cloud, virtual machines, or containers\. Perform the first half of this procedure in the CloudTrail Lake console, then call the [https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html](https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html) API to ingest events, providing your channel ARN and event payload\. After you use the `PutAuditEvents` API to ingest your application activity into CloudTrail, you can use CloudTrail Lake to search, query, and analyze the data that is logged from your applications\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1.  From the navigation pane, open the **Lake** submenu, then choose **Integrations**\. 

1. On the **Add integration** page, enter a name for your channel\. The name can be 3\-128 characters\. Only letters, numbers, periods, underscores, and dashes are allowed\.

1. Choose **My custom integration**\.

1. From **Event delivery location**, choose to log the same activity events to existing event data stores, or create a new event data store\.

   If you choose to create a new event data store, enter a name for the event data store and specify the retention period in days\. Valid values for the retention period are integers from 7 to 2557 \(seven years\)\. The event data store retains event data for the specified number of days\. By default, event data is retained for 2557 days\.

   If you choose to log activity events to one or more existing event data stores, choose the event data stores from the list\. The event data stores can only include activity events\. The event type in the console must be **Events from integrations**\. In the API, the `eventCategory` value must be `ActivityAuditLog`\.

1. In **Resource policy**, configure the resource policy for the integration's channel\. Resource policies are JSON policy documents that specify what actions a specified principal can perform on the resource and under what conditions\. The accounts defined as principals in the resource policy can call the `PutAuditEvents` API to deliver events to your channel\.
**Note**  
If you do not create a resource policy for the channel, only the channel owner can call the `PutAuditEvents` API on the channel\.

   1. \(Optional\) Enter a unique external ID to provide an extra layer of protection\. The external ID is a unique string such as an account ID or a randomly generated string, to prevent against confused deputy\. 
**Note**  
If the resource policy includes an external ID, all calls to the `PutAuditEvents` API must include the external ID\. However, if the policy does not define an external ID, you can still call the `PutAuditEvents` API and specify an `externalId` parameter\.

   1. Choose **Add AWS account** to specify each AWS account ID to add as a principal in the resource policy for the channel\.

1. \(Optional\) In the **Tags** area, you can add up to 50 tag key and value pairs to help you identify, sort, and control access to your event data store and channel\. For more information about how to use IAM policies to authorize access to an event data store based on tags, see [Examples: Denying access to create or delete event data stores based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-eds-tags)\. For more information about how you can use tags in AWS, see [Tagging AWS resources](https://docs.aws.amazon.com/general/latest/gr/aws_tagging.html) in the *AWS General Reference*\.

1. When you are ready to create the new integration, choose **Add integration**\. There is no review page\. CloudTrail creates the integration, but to integrate your custom events, you must specify the channel ARN in a [https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html](https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html) request\.

1. Call the `PutAuditEvents` API to ingest your activity events into CloudTrail\. You can add up to 100 activity events \(or up to 1 MB\) per `PutAuditEvents` request\. You'll need the channel ARN that you created in preceding steps, the payload of events that you want CloudTrail to add, and the external ID \(if specified for your resource policy\)\. Be sure that there is no sensitive or personally\-identifying information in event payload before ingesting it into CloudTrail\. Events that you ingest into CloudTrail must follow the [CloudTrail Lake integrations event schema](query-integration-event-schema.md)\.
**Tip**  
Use [AWS CloudShell](https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html) to be sure you are running the most current AWS APIs\.

   The following examples show how to use the put\-audit\-events CLI command\. The \-\-audit\-events and \-\-channel\-arn parameters are required\. You need the ARN of the channel that you created in the preceding steps, which you can copy from the integration details page\. The value of \-\-audit\-events is a JSON array of event objects\. `--audit-events` includes a required ID from the event, the required payload of the event as the value of `EventData`, and an [optional checksum](#event-data-store-integration-custom-checksum) to help validate the integrity of the event after ingestion into CloudTrail\.

   ```
   aws cloudtrail-data put-audit-events \
   --region region \
   --channel-arn $ChannelArn \
   --audit-events \
   id="event_ID",eventData='"{event_payload}"' \
   id="event_ID",eventData='"{event_payload}"',eventDataChecksum="optional_checksum"
   ```

   The following is an example command with two event examples\.

   ```
   aws cloudtrail-data put-audit-events \
   --region us-east-1 \
   --channel-arn arn:aws:cloudtrail:us-east-1:01234567890:channel/EXAMPLE8-0558-4f7e-a06a-43969EXAMPLE \
   --audit-events \
   id="EXAMPLE3-0f1f-4a85-9664-d50a3EXAMPLE",eventData='"{\"eventVersion\":\0.01\",\"eventSource\":\"custom1.domain.com\", ...
   \}"' \
   id="EXAMPLE7-a999-486d-b241-b33a1EXAMPLE",eventData='"{\"eventVersion\":\0.02\",\"eventSource\":\"custom2.domain.com\", ...
   \}"',eventDataChecksum="EXAMPLE6e7dd61f3ead...93a691d8EXAMPLE"
   ```

   The following example command adds the `--cli-input-json` parameter to specify a JSON file \(`custom-events.json`\) of event payload\.

   ```
   aws cloudtrail-data put-audit-events \
   --channel-arn $channelArn \
   --cli-input-json file://custom-events.json \
   --region us-east-1
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
           \"sourceIPAddress\":\"source_IP_address\",\"recipientAccountId\":\"recipient_account_ID\"}",
           "id": "1"
         }
      ]
   }
   ```

### \(Optional\) Calculate a checksum value<a name="event-data-store-integration-custom-checksum"></a>

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
EXAMPLEHjkI8iehvCUCWTIAbNYkOgO/t0YNw+7rrQE=
```

The checksum value becomes the value of `EventDataChecksum` in your `PutAuditEvents` request\. If the checksum doesn't match with the one for the provided event, CloudTrail rejects the event with an `InvalidChecksum` error\.

## Additional information about integration partners<a name="cloudtrail-lake-partner-information"></a>

The table in this section provides the source name for each integration partner and identifies the integration type \(direct or solution\)\.

The information in the **Source name** column is required when calling the `CreateChannel` API\. You specify the source name as the value for the `Source` parameter\.


****  

| Partner name \(console\) | Source name \(API\) | Integration type | 
| --- | --- | --- | 
| My custom integration | Custom | solution | 
| Cloud Storage Security | CloudStorageSecurityConsole | solution | 
| Clumio | Clumio | direct | 
| CrowdStrike | CrowdStrike | solution | 
| CyberArk | CyberArk | solution | 
| GitHub | GitHub | solution | 
| Kong Inc | KongGatewayEnterprise | solution | 
| LaunchDarkly | LaunchDarkly | direct | 
| Netskope | NetskopeCloudExchange | solution | 
| Nordcloud, an IBM Company | IBMMulticloud | direct | 
| MontyCloud | MontyCloud | direct | 
| Okta | OktaSystemLogEvents | solution | 
| One Identity | OneLogin | solution | 
| Shoreline\.io | Shoreline | solution | 
| Snyk\.io | Snyk | direct | 
| Wiz | WizAuditLogs | solution | 

### View partner documentation<a name="lake-integration-partner-documentation"></a>

You can learn more about a partner's integration with CloudTrail Lake by viewing their documentation\.

**To view partner documentation**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1.  From the navigation pane, open the **Lake** submenu, then choose **Integrations**\. 

1. From the **Integrations** page, choose **Available sources**, then choose **Learn more** for the partner whose documentation you want to view\. 