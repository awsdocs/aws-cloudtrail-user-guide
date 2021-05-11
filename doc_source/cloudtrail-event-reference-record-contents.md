# CloudTrail Record Contents<a name="cloudtrail-event-reference-record-contents"></a>

The body of the record contains fields that help you determine the requested action as well as when and where the request was made\. When the value of **Optional** is **True**, the field is only present when it applies to the service, API, or event type\. An **Optional** value of **False** means that the field is either always present, or that its presence does not depend on the service, API, or event type\. An example is `responseElements`, which is present in events for actions that make changes \(create, update, or delete actions\)\.

**`eventTime`**  
The date and time the request was made, in coordinated universal time \(UTC\)\. An event's time stamp comes from the local host that provides the service API endpoint on which the API call was made\. For example, a CreateBucket API event that is run in the US West \(Oregon\) Region would get its time stamp from the time on an AWS host running the Amazon S3 endpoint, `s3.us-west-2.amazonaws.com`\. In general, AWS services use Network Time Protocol \(NTP\) to synchronize their system clocks\.  
**Since:** 1\.0  
**Optional:** False

**`eventVersion`**  
The version of the log event format\. The current version is 1\.08\.  
The `eventVersion` value is a major and minor version in the form *major\_version*\.*minor\_version*\. For example, you can have an `eventVersion` value of `1.07`, where `1` is the major version, and `07` is the minor version\.  
CloudTrail increments the major version if a change is made to the event structure that is not backward\-compatible\. This includes removing a JSON field that already exists, or changing how the contents of a field are represented \(for example, a date format\)\. CloudTrail increments the minor version if a change adds new fields to the event structure\. This can occur if new information is available for some or all existing events, or if new information is available only for new event types\. Applications can ignore new fields to stay compatible with new minor versions of the event structure\.  
If CloudTrail introduces new event types, but the structure of the event is otherwise unchanged, the event version does not change\.  
To be sure that your applications can parse the event structure, we recommend that you perform an equal\-to comparison on the major version number\. To be sure that fields that are expected by your application exist, we also recommend performing a greater\-than\-or\-equal\-to comparison on the minor version\. There are no leading zeroes in the minor version\. You can interpret both *major\_version* and *minor\_version* as numbers, and perform comparison operations\.  
**Since:** 1\.0  
**Optional:** False

**`userIdentity`**  
Information about the user that made a request\. For more information, see [CloudTrail userIdentity Element](cloudtrail-event-reference-user-identity.md)\.   
**Since:** 1\.0  
**Optional:** False

**`eventSource`**  
The service that the request was made to\. This name is typically a short form of the service name without spaces plus `.amazonaws.com`\. For example:  
+ AWS CloudFormation is `cloudformation.amazonaws.com`\.
+ Amazon EC2 is `ec2.amazonaws.com`\.
+ Amazon Simple Workflow Service is `swf.amazonaws.com`\.
This convention has some exceptions\. For example, the `eventSource` for Amazon CloudWatch is `monitoring.amazonaws.com`\.  
**Since:** 1\.0  
**Optional:** False

**`eventName`**  
The requested action, which is one of the actions in the API for that service\.  
**Since:** 1\.0  
**Optional:** False

**`awsRegion`**  
The AWS region that the request was made to, such as `us-east-2`\. See [CloudTrail Supported Regions](cloudtrail-supported-regions.md)\.  
**Since:** 1\.0  
**Optional:** False

**`sourceIPAddress`**  
The IP address that the request was made from\. For actions that originate from the service console, the address reported is for the underlying customer resource, not the console web server\. For services in AWS, only the DNS name is displayed\.  
**Since:** 1\.0  
**Optional:** False

**`userAgent`**  
The agent through which the request was made, such as the AWS Management Console, an AWS service, the AWS SDKs or the AWS CLI\. This field has a maximum size of 1 KB; content exceeding that limit is truncated\. The following are example values:  
+ `signin.amazonaws.com` – The request was made by an IAM user with the AWS Management Console\.
+ `console.amazonaws.com `– The request was made by a root user with the AWS Management Console\.
+ `lambda.amazonaws.com` – The request was made with AWS Lambda\.
+ `aws-sdk-java` – The request was made with the AWS SDK for Java\. 
+ `aws-sdk-ruby` – The request was made with the AWS SDK for Ruby\. 
+ `aws-cli/1.3.23 Python/2.7.6 Linux/2.6.18-164.el5` – The request was made with the AWS CLI installed on Linux\. 
For events originated by AWS, this field is usually `AWS Internal/#`, where `#` is a number used for internal purposes\.
**Since:** 1\.0  
**Optional:** False

**`errorCode`**  
The AWS service error if the request returns an error\. For an example that shows this field, see [Error Code and Message Log Example](cloudtrail-log-file-examples.md#error-code-and-error-message)\. This field has a maximum size of 1 KB; content exceeding that limit is truncated\.  
**Since:** 1\.0  
**Optional:** True

**`errorMessage`**  
If the request returns an error, the description of the error\. This message includes messages for authorization failures\. CloudTrail captures the message logged by the service in its exception handling\. For an example, see [Error Code and Message Log Example](cloudtrail-log-file-examples.md#error-code-and-error-message)\. This field has a maximum size of 1 KB; content exceeding that limit is truncated\.  
Some AWS services provide the `errorCode` and `errorMessage` as top\-level fields in the event\. Other AWS services provide error information as part of `responseElements`\.
**Since:** 1\.0  
**Optional:** True

**`requestParameters`**  
The parameters, if any, that were sent with the request\. These parameters are documented in the API reference documentation for the appropriate AWS service\. This field has a maximum size of 100 KB; content exceeding that limit is truncated\.  
**Since:** 1\.0  
**Optional:** False

**`responseElements`**  
The response element for actions that make changes \(create, update, or delete actions\)\. If an action does not change state \(for example, a request to get or list objects\), this element is omitted\. These actions are documented in the API reference documentation for the appropriate AWS service\. This field has a maximum size of 100 KB; content exceeding that limit is truncated\.  
The `responseElements` value is useful to help you trace a request with AWS Support\. Both `x-amz-request-id` and `x-amz-id-2` contain information that helps you trace a request with AWS Support\. These values are the same as those that the service returns in the response to the request that initiates the events, so you can use them to match the event to the request\.  
**Since:** 1\.0  
**Optional:** False

 **`additionalEventData`**   
Additional data about the event that was not part of the request or response\. This field has a maximum size of 28 KB; content exceeding that limit is truncated\.  
Support for this field begins with `eventVersion` `1.00`\.  
**Since:** 1\.0  
**Optional:** True

**`requestID`**  
The value that identifies the request\. The service being called generates this value\. This field has a maximum size of 1 KB; content exceeding that limit is truncated\.  
Support for this field begins with `eventVersion` `1.01`\.  
**Since:** 1\.01  
**Optional:** False

**`eventID`**  
GUID generated by CloudTrail to uniquely identify each event\. You can use this value to identify a single event\. For example, you can use the ID as a primary key to retrieve log data from a searchable database\.   
**Since:** 1\.01  
**Optional:** False

**`eventType`**  
Identifies the type of event that generated the event record\. This can be the one of the following values:   
+ `AwsApiCall` – An API was called\. 
+ `AwsServiceEvent` – The service generated an event related to your trail\. For example, this can occur when another account made a call with a resource that you own\. 
+ `AwsConsoleSignin` – A user in your account \(root, IAM, federated, SAML, or SwitchRole\) signed in to the AWS Management Console\.
**Since:** 1\.02  
**Optional:** False

**`apiVersion`**  
Identifies the API version associated with the `AwsApiCall` `eventType` value\.  
**Since:** 1\.01  
**Optional:** True

**`managementEvent`**  
A Boolean value that identifies whether the event is a management event\. `managementEvent` is shown in an event record if `eventVersion` is 1\.06 or higher, and the event type is one of the following:  
+ `AwsApiCall`
+ `AwsConsoleAction`
+ `AwsConsoleSignIn`
+ `AwsServiceEvent`
**Since:** 1\.06  
**Optional:** True

 **`readOnly`**   
Identifies whether this operation is a read\-only operation\. This can be one of the following values:  
+ `true` – The operation is read\-only \(for example, `DescribeTrails`\)\.
+ `false` – The operation is write\-only \(for example, `DeleteTrail`\)\.
````  
**Since:** 1\.01  
**Optional:** True

 **`resources`**   
A list of resources accessed in the event\. The field can contain the following information:  
+ Resource ARNs
+ Account ID of the resource owner
+ Resource type identifier in the format: `AWS::aws-service-name::data-type-name`
For example, when an `AssumeRole` event is logged, the `resources` field can appear like the following:  
+ ARN: `arn:aws:iam::123456789012:role/myRole`
+ Account ID: `123456789012`
+ Resource type identifier: `AWS::IAM::Role`
For example logs with the `resources` field, see [AWS STS API Event in CloudTrail Log File](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html#stscloudtrailexample) in the *IAM User Guide* or [ Logging AWS KMS API Calls](https://docs.aws.amazon.com/kms/latest/developerguide/logging-using-cloudtrail.html) in the *AWS Key Management Service Developer Guide*\.  
**Since:** 1\.01  
**Optional:** True

**`recipientAccountId`**  
Represents the account ID that received this event\. The `recipientAccountID` may be different from the [CloudTrail userIdentity Element](cloudtrail-event-reference-user-identity.md) `accountId`\. This can occur in cross\-account resource access\. For example, if a KMS key, also known as a [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html), was used by a separate account to call the [Encrypt API](https://docs.aws.amazon.com/kms/latest/developerguide/ct-encrypt.html), the `accountId` and `recipientAccountID` values will be the same for the event delivered to the account that made the call, but the values will be different for the event that is delivered to the account that owns the CMK\.  
**Since:** 1\.02  
**Optional:** True

**`serviceEventDetails`**  
Identifies the service event, including what triggered the event and the result\. For more information, see [AWS Service Events](non-api-aws-service-events.md)\. This field has a maximum size of 100 KB; content exceeding that limit is truncated\.  
**Since:** 1\.05  
**Optional:** True

**`sharedEventID`**  
GUID generated by CloudTrail to uniquely identify CloudTrail events from the same AWS action that is sent to different AWS accounts\.  
For example, when an account uses a KMS key, also known as a [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html), that belongs to another account, the account that used the CMK and the account that owns the CMK receive separate CloudTrail events for the same action\. Each CloudTrail event delivered for this AWS action shares the same `sharedEventID`, but also has a unique `eventID` and `recipientAccountID`\.  
For more information, see [sharedEventID Example](shared-event-ID.md)\.  
The `sharedEventID` field is present only when CloudTrail events are delivered to multiple accounts\. If the caller and owner are the same AWS account, CloudTrail sends only one event, and the `sharedEventID` field is not present\.
**Since:** 1\.03  
**Optional:** True

 **`vpcEndpointId`**   
Identifies the VPC endpoint in which requests were made from a VPC to another AWS service, such as Amazon S3\.   
**Since:** 1\.04  
**Optional:** True

**`eventCategory`**  
Shows the event category that is used in [https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_LookupEvents.html](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_LookupEvents.html) calls\.  
+ For management events, the value is `Management`\.
+ For data events, the value is `Data`\.
+ For Insights events, the value is `Insight`\.
**Since:** 1\.07  
**Optional:** False

**`addendum`**  
If an event delivery was delayed, or additional information about an existing event becomes available after the event is logged, an addendum field shows information about why the event was delayed\. If information was missing from an existing event, the addendum field includes the missing information and a reason for why it was missing\. Contents include the following\.  
+ **`reason`** \- The reason that the event or some of its contents were missing\. Values can be any of the following\.
  + **`DELIVERY_DELAY`** – There was a delay delivering events\. This could be caused by high network traffic, connectivity issues, or a CloudTrail service issue\.
  + **`UPDATED_DATA`** – A field in the event record was missing or had an incorrect value\.
  + **`SERVICE_OUTAGE`** – A service that logs events to CloudTrail had an outage, and couldn’t log events to CloudTrail\. This is exceptionally rare\.
+ **`updatedFields`** \- The event record fields that are updated by the addendum\. This is only provided if the reason is `UPDATED_DATA`\.
+ **`originalRequestID`** \- The original unique ID of the request\. This is only provided if the reason is `UPDATED_DATA`\.
+ **`originalEventID`** \- The original event ID\. This is only provided if the reason is `UPDATED_DATA`\.
**Since:** 1\.08  
**Optional:** True

**`sessionCredentialFromConsole`**  
Shows whether or not an event originated from a AWS Management Console session\. This field is not shown unless the value is `true`, meaning that the client that was used to make the API call was either a proxy or an external client\. If a proxy client was used, the `tlsDetails` event field is not shown\.  
**Since:** 1\.08  
**Optional:** True

**`edgeDeviceDetails`**  
Shows information about edge devices that are targets of a request\. Currently, [http://aws.amazon.com/s3/outposts/](http://aws.amazon.com/s3/outposts/) device events include this field\. This field has a maximum size of 28 KB; content exceeding that limit is truncated\.  
**Since:** 1\.08  
**Optional:** True

**`tlsDetails`**  
Shows information about the Transport Layer Security \(TLS\) version, cipher suites, and the FQDN of the client\-provided host name of a service API call\. Contents include the following\. CloudTrail still logs partial TLS details if expected information is missing or empty\. For example, if the TLS version and cipher suite are present, but the `HOST` header is empty, available TLS details are still logged in the CloudTrail event\.  
If `sessionCredentialFromConsole` is present with a value of `true`, `tlsDetails` is present in an event record only if an external client was used to make the API call\.  
+ **`tlsVersion`** \- The TLS version of a request\.
+ **`cipherSuite`** \- The cipher suite \(combination of security algorithms used\) of a request\.
+ **`clientProvidedHostHeader`** \- The FQDN of the client that made the request\.
**Since:** 1\.08  
**Optional:** True

## Insights Event Record Fields<a name="eventrecord-insight"></a>

The following are attributes shown in the JSON structure of an Insights event that differ from those in a management or data event\.

**`sharedEventId`**  
A `sharedEventID` for CloudTrail Insights events differs from the `sharedEventID` for the management and data types of CloudTrail events\. In Insights events, a `sharedEventID` is a GUID that is generated by CloudTrail Insights to uniquely identify an Insights event\. `sharedEventID` is common between the start and the end Insights events, and helps to connect both events to uniquely identify unusual activity\. You can think of the `sharedEventID` as the overall Insights event ID\.  
**Since:** 1\.07  
**Optional:** False

**`insightDetails`**  
Insights events only\. Shows information about the underlying triggers of an Insights event, such as event source, user agent, statistics, API name, and whether the event is the start or end of the Insights event\. For more information about the contents of the `insightDetails` block, see [CloudTrail Insights `insightDetails` element](cloudtrail-event-reference-insight-details.md)\.  
**Since:** 1\.07  
**Optional:** False