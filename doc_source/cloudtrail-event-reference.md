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

There are two events logged to show unusual activity in CloudTrail Insights: a start event and an end event\. The following example shows a single log record of a starting Insights event that occurred when the Application Auto Scaling API `CompleteLifecycleAction` was called an unusual number of times\. For Insights events, the value of `eventCategory` is `Insight`\. An `insightDetails` block identifies the event state, source, name, Insights type, and context, including statistics and attributions\. For more information about the `insightDetails` block, see [CloudTrail Insights `insightDetails` element](cloudtrail-event-reference-insight-details.md)\.

```
{
          "eventVersion": "1.07",
          "eventTime": "2020-07-21T20:56:00Z",
          "awsRegion": "us-east-1",
          "eventID": "abcd00b0-ccfe-422d-961c-98a2198a408x",
          "eventType": "AwsCloudTrailInsight",
          "recipientAccountId": "838185438692",
          "sharedEventID": "7bb000gg-22b3-4c03-94af-c74tj0c8m7c0",
          "insightDetails": {
            "state": "Start",
            "eventSource": "autoscaling.amazonaws.com",
            "eventName": "CompleteLifecycleAction",
            "insightType": "ApiCallRateInsight",
            "insightContext": {
              "statistics": {
                "baseline": {
                  "average": 0.0000882145
                },
                "insight": {
                  "average": 0.6
                },
                "insightDuration": 5,
                "baselineDuration": 11336
              },
              "attributions": [
                {
                  "attribute": "userIdentityArn",
                  "insight": [
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole1",
                      "average": 0.2
                    },
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole2",
                      "average": 0.2
                    },
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole3",
                      "average": 0.2
                    }
                  ],
                  "baseline": [
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole1",
                      "average": 0.0000882145
                    }
                  ]
                },
                {
                  "attribute": "userAgent",
                  "insight": [
                    {
                      "value": "codedeploy.amazonaws.com",
                      "average": 0.6
                    }
                  ],
                  "baseline": [
                    {
                      "value": "codedeploy.amazonaws.com",
                      "average": 0.0000882145
                    }
                  ]
                },
                {
                  "attribute": "errorCode",
                  "insight": [
                    {
                      "value": "null",
                      "average": 0.6
                    }
                  ],
                  "baseline": [
                    {
                      "value": "null",
                      "average": 0.0000882145
                    }
                  ]
                }
              ]
            }
          },
          "eventCategory": "Insight"
}
```

The following topics list the data fields that CloudTrail captures for each AWS API call and sign\-in event\. 

**Topics**
+ [CloudTrail Record Contents](cloudtrail-event-reference-record-contents.md)
+ [CloudTrail userIdentity Element](cloudtrail-event-reference-user-identity.md)
+ [CloudTrail Insights `insightDetails` element](cloudtrail-event-reference-insight-details.md)
+ [Non\-API Events Captured by CloudTrail](cloudtrail-non-api-events.md)