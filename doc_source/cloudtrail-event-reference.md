# CloudTrail Log Event Reference<a name="cloudtrail-event-reference"></a>

A CloudTrail log is a record in JSON format\. The log contains information about requests for resources in your account, such as who made the request, the services used, the actions performed, and parameters for the action\. The event data is enclosed in a `Records` array\. 

The following example shows a single log record at the beginning of a log file\. The entry shows that an IAM user named Alice called the CloudTrail `StartLogging` API from the CloudTrail console to start the logging process\. 

```
{
	"Records": [{
		"eventVersion": "1.01",
		"userIdentity": {
			"type": "IAMUser",
			"principalId": "AIDAJDPLRKLG7UEXAMPLE",
			"arn": "arn:aws:iam::123456789012:user/Alice",
			"accountId": "123456789012",
			"accessKeyId": "AKIAIOSFODNN7EXAMPLE",
			"userName": "Alice",
			"sessionContext": {
				"attributes": {
					"mfaAuthenticated": "false",
					"creationDate": "2014-03-18T14:29:23Z"
				}
			}
		},
		"eventTime": "2014-03-18T14:30:07Z",
		"eventSource": "cloudtrail.amazonaws.com",
		"eventName": "StartLogging",
		"awsRegion": "us-west-2",
		"sourceIPAddress": "72.21.198.64",
		"userAgent": "signin.amazonaws.com",
		"requestParameters": {
			"name": "Default"
		},
		"responseElements": null,
		"requestID": "cdc73f9d-aea9-11e3-9d5a-835b769c0d9c",
		"eventID": "3074414d-c626-42aa-984b-68ff152d6ab7"
	},
    ... additional entries ...
	]
```

The following topics list the data fields that CloudTrail captures for each AWS API call and sign\-in event\. 


+ [CloudTrail Record Contents](cloudtrail-event-reference-record-contents.md)
+ [CloudTrail userIdentity Element](cloudtrail-event-reference-user-identity.md)
+ [Non\-API Events Captured by CloudTrail](cloudtrail-non-api-events.md)