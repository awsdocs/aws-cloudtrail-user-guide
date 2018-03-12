# AWS Console Sign\-in Events<a name="cloudtrail-event-reference-aws-console-sign-in-events"></a>

CloudTrail records attempts to sign into the AWS Management Console, the AWS Discussion Forums and the AWS Support Center\. All IAM user sign\-in attempts \(successes and failures\), all federated user sign\-in events \(successes and failures\) and all successful AWS root account sign\-in attempts generate records in CloudTrail log files\. Note, however, that CloudTrail does not record root sign\-in failures\. 

The following record shows that an IAM user named Alice successfully signed into the AWS console without using multi\-factor authentication\. 

```
{
   "Records":[
      {
         "eventVersion":"1.02",
         "userIdentity":{
            "type":"IAMUser",
            "principalId":"AIDAELOPP77CWZEXAMPLE",
            "arn":"arn:aws:iam::12345679012:user/alice",
            "accountId":"12345679012",
            "userName":"alice"
         },
         "eventTime":"2014-07-08T17:35:32Z",
         "eventSource":"signin.amazonaws.com",
         "eventName":"ConsoleLogin",
         "awsRegion":"us-east-2",
         "sourceIPAddress":"192.0.2.0",
         "userAgent":"Mozilla/5.0 (Windows; U; Windows NT 5.0; en-US; rv:1.4b) Gecko/20030516 Mozilla Firebird/0.6",
         "requestParameters":null,
         "responseElements":{
            "ConsoleLogin":"Success"
         },
         "additionalEventData":{
            "MobileVersion":"No",
            "LoginTo":"https://console.aws.amazon.com/sns",
            "MFAUsed":"No"
         },
         "eventID":"3fcfb182-98f8-4744-bd45-10a395ab61cb",
         "eventType": "AwsConsoleSignin"
      }
   ]
}
```

The following record shows that an IAM user named Alice logged into the AWS console by using multi\-factor authentication\. 

```
{

   "Records":[
      {
         "eventVersion":"1.02",
         "userIdentity":{
            "type":"IAMUser",
            "principalId":"AIDAEZ7VBM6PDZEXAMPLE",
            "arn":"arn:aws:iam::12345679012:user/Alice",
            "accountId":"12345679012",
            "userName":"Alice"
         },
         "eventTime":"2014-07-08T17:36:03Z",
         "eventSource":"signin.amazonaws.com",
         "eventName":"ConsoleLogin",
         "awsRegion":"us-east-2",
         "sourceIPAddress":"192.0.2.0",
         "userAgent":"Mozilla/5.0 (Windows; U; Windows NT 5.0; en-US; rv:1.4b) Gecko/20030516 Mozilla Firebird/0.6",
         "requestParameters":null,
         "responseElements":{
            "ConsoleLogin":"Success"
         },
         "additionalEventData":{
            "MobileVersion":"Yes",
            "LoginTo":"https://console.aws.amazon.com/sns",
            "MFAUsed":"Yes"
         },
         "eventID":"5d2c2f55-3d1e-4336-b940-dbf8e66f588c",
         "eventType": "AwsConsoleSignin"
      }
   ]
}
```

The following record shows an unsuccessful AWS console sign\-in attempt because of an authentication failure\. 

```
{
   "Records":[ 
      {
         "eventVersion":"1.02",
         "userIdentity":{
            "type":"IAMUser",
            "principalId":"AIDAELOPP77CWZEXAMPLE",
            "accountId":"12345679012",
            "accessKeyId":"",
            "userName":"alice"
         },
         "eventTime":"2014-07-08T17:35:27Z",
         "eventSource":"signin.amazonaws.com",
         "eventName":"ConsoleLogin",
         "awsRegion":"us-east-2",
         "sourceIPAddress":"192.0.2.0",
         "userAgent":"Mozilla/5.0 (Windows; U; Windows NT 5.0; en-US; rv:1.4b) Gecko/20030516 Mozilla Firebird/0.6",
         "errorMessage":"Failed authentication",
         "requestParameters":null,
         "responseElements":{
            "ConsoleLogin":"Failure"
         },
         "additionalEventData":{
            "MobileVersion":"No",
            "LoginTo":"https://console.aws.amazon.com/sns",
            "MFAUsed":"No"
         },
         "eventID":"11ea990b-4678-4bcd-8fbe-62509088b7cf",
         "eventType": "AwsConsoleSignin"
      }
   ]
}
```