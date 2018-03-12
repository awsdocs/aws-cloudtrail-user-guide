# AWS Service Events<a name="non-api-aws-service-events"></a>

CloudTrail supports logging non\-API service events to your Amazon S3 bucket\. These events are related to AWS services but are not directly triggered by a request to a public AWS API\. For these events, the `eventType` field is `AwsServiceEvent`\. The following is an example scenario of an AWS service event\.

****

1. You want to run a Spot Instance for your application and submit a bid for a specified number and type of EC2 instances\.

1.  Your bid price exceeds the current Spot price, and the EC2 instances are created for you\. 

1. When the Spot price exceeds your bid price, your EC2 Spot Instances are terminated and given to other customers\.

In the example, CloudTrail logs the service event activity to your Amazon S3 bucket\. One event shows that the EC2 Spot Instance was created and another event shows when the EC2 Spot Instance was terminated\. For information related to the event, see the `serviceEventDetails` field\.

The following example event shows that a bid was accepted for an EC2 Spot Instance\. The instance ID appears in the `serviceEventDetails` field\.

```
{
      "eventVersion": "1.05",
      "userIdentity": {
        "accountId": "123456789012",
        "invokedBy": "ec2.amazonaws.com"
      },
      "eventTime": "2016-08-16T22:30:00Z",
      "eventSource": "ec2.amazonaws.com",
      "eventName": "BidFulfilledEvent",
      "userAgent": "ec2.amazonaws.com",
      "sourceIPAddress": "ec2.amazonaws.com",
      "awsRegion": "us-east-2",
      "eventID": "d27a6096-807b-4bd0-8c20-a33a83375055",
      "eventType": "AwsServiceEvent",
      "recipientAccountId": "123456789012",
      "RequestParameters": null,
      "ResponseElements": null,
      "serviceEventDetails": {
        "instanceIdSet": [
          "i-04cf7ed6b11ccfac5"
        ]
      }
    }
```

The following example event shows that the EC2 Spot Instance was terminated when the Spot price exceeded your bid price\. The instance ID appears in the `serviceEventDetails` field\.

```
{
      "eventVersion": "1.05",
      "userIdentity": {
        "accountId": "123456789012",
        "invokedBy": "ec2.amazonaws.com"
      },
      "eventTime": "2016-08-16T22:30:00Z",
      "eventSource": "ec2.amazonaws.com",
      "userAgent": "ec2.amazonaws.com",
      "sourceIPAddress": "ec2.amazonaws.com",
      "eventName": "BidEvictedEvent",
      "awsRegion": "us-east-2",
      "eventID": "d27a6096-807b-4bd0-8c20-a33a83375054",
     "eventType": "AwsServiceEvent",
      "recipientAccountId": "123456789012",
      "RequestParameters": null,
      "ResponseElements": null,
      "serviceEventDetails": {
        "instanceIdSet": [
          "i-1eb2ac8e"
        ]
      }
    }
```