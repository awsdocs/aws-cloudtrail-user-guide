# sharedEventID Example<a name="shared-event-ID"></a>

The following is an example that describes how CloudTrail delivers two events for the same action:

1.  Alice has AWS account \(111111111111\) and creates a customer master key \(CMK\)\. She is the owner of this CMK\. 

1.  Bob has AWS account \(222222222222\)\. Alice gives Bob permission to use the CMK\. 

1.  Each account has a trail and a separate bucket\.

1.  Bob uses the CMK to call the `Encrypt` API\. 

1.  CloudTrail sends two separate events\. 

   + One event is sent to Bob\. The event shows that he used the CMK\.

   + One event is sent to Alice\. The event shows that Bob used the CMK\.

   + The events have the same `sharedEventID`, but the `eventID` and `recipientAccountID` are unique\.

![\[How the sharedEventID field appears in logs\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/event-reference-sharedEventId.png)![\[How the sharedEventID field appears in logs\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)![\[How the sharedEventID field appears in logs\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)