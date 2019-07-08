# CloudTrail Log Event Reference<a name="cloudtrail-event-reference"></a>

A CloudTrail log is a record in JSON format\. The log contains information about requests for resources in your account, such as who made the request, the services used, the actions performed, and parameters for the action\. The event data is enclosed in a `Records` array\. 

The following example shows a single log record of an event where an IAM user named Mary\_Major called the CloudTrail `StartLogging` API from the CloudTrail console to start the logging process\. 

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAJDPLRKLG7UEXAMPLE",
        "arn": "arn:aws:iam::123456789012:user/Mary_Major",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "Mary_Major",
        "sessionContext": {
            "sessionIssuer": {},
            "webIdFederationData": {},
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2019-06-18T22:28:31Z"
            }
        },
        "invokedBy": "signin.amazonaws.com"
    },
    "eventTime": "2019-06-19T00:18:31Z",
    "eventSource": "cloudtrail.amazonaws.com",
    "eventName": "StartLogging",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "203.0.113.64",
    "userAgent": "signin.amazonaws.com",
    "requestParameters": {
        "name": "arn:aws:cloudtrail:us-east-2:123456789012:trail/My-First-Trail"
    },
    "responseElements": null,
    "requestID": "ddf5140f-EXAMPLE",
    "eventID": "7116c6a1-EXAMPLE",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
},
    ... additional entries ...
```

The following topics list the data fields that CloudTrail captures for each AWS API call and sign\-in event\. 

**Topics**
+ [CloudTrail Record Contents](cloudtrail-event-reference-record-contents.md)
+ [CloudTrail userIdentity Element](cloudtrail-event-reference-user-identity.md)
+ [Non\-API Events Captured by CloudTrail](cloudtrail-non-api-events.md)