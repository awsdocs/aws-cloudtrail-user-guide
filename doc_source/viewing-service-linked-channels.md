# Viewing service\-linked channels for CloudTrail by using the AWS CLI<a name="viewing-service-linked-channels"></a>

AWS services can create a service\-linked channel to receive CloudTrail events on your behalf\. The AWS service creating the service\-linked channel configures advanced event selectors for the channel and specifies whether the channel applies to all regions, or the current region\.

Using the AWS CLI, you can view information about any CloudTrail service\-linked channels created by AWS services\.

**Topics**
+ [Get a CloudTrail service\-linked channel](#get-slc)
+ [List all CloudTrail service\-linked channels](#view-all-slc)
+ [AWS service events on service\-linked channels](#slc-service-events)

## Get a CloudTrail service\-linked channel<a name="get-slc"></a>

The following example AWS CLI command returns information about a specific CloudTrail service\-linked channel, including the name of the destination AWS service, any advanced selectors configured for the channel, and whether the channel applies to all regions or a single region\.

You must specify an ARN or the ID suffix of an ARN for `--channel`\.

```
aws cloudtrail get-channel --channel EXAMPLE-ee54-4813-92d5-999aeEXAMPLE 
```

The following is an example response\. In this example, `AWS_service_name` represents the name of the AWS service that created the channel\.

```
{
    "ChannelArn": "arn:aws:cloudtrail:us-east-1:111122223333:channel/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
    "Name": "aws-service-channel/AWS_service_name/slc",
    "Source": "CloudTrail",
    "SourceConfig": {
        "ApplyToAllRegions": false,
        "AdvancedEventSelectors": [
            {
                "Name": "Management Events Only",
                "FieldSelectors": [
                    {
                        "Field": "eventCategory",
                        "Equals": [
                            "Management"
                        ]
                    }
                ]
            }
        ]
    },
    "Destinations": [
        {
            "Type": "AWS_SERVICE",
            "Location": "AWS_service_name"
        }
    ]
}
```

## List all CloudTrail service\-linked channels<a name="view-all-slc"></a>

The following example AWS CLI command returns information about all CloudTrail service\-linked channels that were created on your behalf\. Optional parameters include `--max-results`, to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail list-channels
```

The following is an example response\. In this example, `AWS_service_name` represents the name of the AWS service that created the channel\.

```
{
    "Channels": [
        {
            "ChannelArn": "arn:aws:cloudtrail:us-east-1:111122223333:channel/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
            "Name": "aws-service-channel/AWS_service_name/slc"
        }
    ]
}
```

## AWS service events on service\-linked channels<a name="slc-service-events"></a>

The AWS service managing the service\-linked channel can initiate actions on the service\-linked channel \(for example, creating or updating a service\-linked channel\)\. CloudTrail logs these actions as [AWS service events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/non-api-aws-service-events.html), and delivers these events to the **Event history**, and any active trails and event data stores configured for management events\. For these events, the `eventType` field is `AwsServiceEvent`\. 

The following is an example log file entry of an AWS service event for creation of a service\-linked channel\. 

```
{
   "eventVersion":"1.08",
   "userIdentity":{
      "accountId":"111122223333",
      "invokedBy":"AWS Internal"
   },
   "eventTime":"2022-08-18T17:11:22Z",
   "eventSource":"cloudtrail.amazonaws.com",
   "eventName":"CreateServiceLinkedChannel",
   "awsRegion":"us-east-1",
   "sourceIPAddress":"AWS Internal",
   "userAgent":"AWS Internal",
   "requestParameters":null,
   "responseElements":null,
   "requestID":"564f004c-EXAMPLE",
   "eventID":"234f004b-EXAMPLE",
   "readOnly":false,
   "resources":[
      {
         "accountId":"184434908391",
         "type":"AWS::CloudTrail::Channel",
         "ARN":"arn:aws:cloudtrail:us-east-1:111122223333:channel/7944f0ec-EXAMPLE"
      }
   ],
   "eventType":"AwsServiceEvent",
   "managementEvent":true,
   "recipientAccountId":"111122223333",
   "eventCategory":"Management"
}
```