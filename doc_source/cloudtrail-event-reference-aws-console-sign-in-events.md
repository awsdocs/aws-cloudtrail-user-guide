# AWS Console Sign\-in Events<a name="cloudtrail-event-reference-aws-console-sign-in-events"></a>

CloudTrail records attempts to sign into the AWS Management Console, the AWS Discussion Forums, and the AWS Support Center\. All IAM user and root user sign\-in events, as well as all federated user sign\-in events, generate records in CloudTrail log files\. 

## Example Records for IAM Users<a name="cloudtrail-event-reference-aws-console-sign-in-events-iam-user"></a>

The following record shows that an IAM user named Anaya successfully signed into the AWS Management Console without using multi\-factor authentication\. 

```
{
   "Records":[
      {
         "eventVersion":"1.05",
         "userIdentity":{
            "type":"IAMUser",
            "principalId":"AIDACKCEVSQ6C2EXAMPLE",
            "arn":"arn:aws:iam::111122223333:user/anaya",
            "accountId":"111122223333",
            "userName":"anaya"
         },
         "eventTime":"2018-08-29T16:24:34Z",
         "eventSource":"signin.amazonaws.com",
         "eventName":"ConsoleLogin",
         "awsRegion":"us-east-2",
         "sourceIPAddress":"192.0.2.0",
         "userAgent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:62.0) Gecko/20100101 Firefox/62.0",
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

The following record shows that an IAM user named Anaya logged into the AWS Management Console using multi\-factor authentication \(MFA\)\. 

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "arn": "arn:aws:iam::111122223333:user/anaya",
        "accountId": "111122223333",
        "userName": "anaya"
    },
    "eventTime": "2018-07-24T18:34:07Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "ConsoleLogin",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36",
    "requestParameters": null,
    "responseElements": {
        "ConsoleLogin": "Success"
    },
    "additionalEventData": {
        "LoginTo": "https://console.aws.amazon.com/console/home?state=hashArgs%23&isauthcode=true",
        "MobileVersion": "No",
        "MFAUsed": "Yes"
    },
    "eventID": "fed06f42-cb12-4764-8c69-121063dc79b9",
    "eventType": "AwsConsoleSignIn",
    "recipientAccountId": "111122223333"
}
```

The following record shows an IAM user's unsuccessful sign\-in attempt\. 

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "accountId": "111122223333",
        "accessKeyId": "",
        "userName": "anaya"
    },
    "eventTime": "2018-07-24T18:32:11Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "ConsoleLogin",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36",
    "errorMessage": "Failed authentication",
    "requestParameters": null,
    "responseElements": {
        "ConsoleLogin": "Failure"
    },
    "additionalEventData": {
        "LoginTo": "https://console.aws.amazon.com/console/home?state=hashArgs%23&isauthcode=true",
        "MobileVersion": "No",
        "MFAUsed": "Yes"
    },
    "eventID": "d38ce1b3-4575-4cb8-a632-611b8243bfc3",
    "eventType": "AwsConsoleSignIn",
    "recipientAccountId": "111122223333"
}
```

The following shows that the sign\-process checked whether multi\-factor authentication \(MFA\) is required for an IAM user during sign\-in\.

```
 {
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "accountId": "111122223333",
        "accessKeyId": "",
        "userName": "anaya"
    },
    "eventTime": "2018-07-24T18:33:45Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "CheckMfa",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36",
    "requestParameters": null,
    "responseElements": {
        "CheckMfa": "Success"
    },
    "additionalEventData": {
        "MfaType": "U2F MFA"
    },
    "eventID": "f8ef8fc5-e3e8-4ee1-9d52-2f412ddf17e3",
    "eventType": "AwsConsoleSignIn",
    "recipientAccountId": "111122223333"
}
```

## Example Records for Root Users<a name="cloudtrail-event-reference-aws-console-sign-in-events-root"></a>

The following shows a successful sign\-in event for a root user not using MFA\.

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "Root",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "arn": "arn:aws:iam::111122223333:root",
        "accountId": "111122223333",
        "accessKeyId": ""
    },
    "eventTime": "2018-08-29T16:24:34Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "ConsoleLogin",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:62.0) Gecko/20100101 Firefox/62.0",
    "requestParameters": null,
    "responseElements": {
        "ConsoleLogin": "Success"
    },
    "additionalEventData": {
        "LoginTo": "https://console.aws.amazon.com/console/home?state=hashArgs%23&isauthcode=true",
        "MobileVersion": "No",
        "MFAUsed": "No"
    },
    "eventID": "deb1e1f9-c99b-4612-8e9f-21f93b5d79c0",
    "eventType": "AwsConsoleSignIn",
    "recipientAccountId": "111122223333"
}
```

The following shows a failed sign\-in event for a root user not using MFA\.

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "Root",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "arn": "arn:aws:iam::111122223333:root",
        "accountId": "111122223333",
        "accessKeyId": ""
    },
    "eventTime": "2018-08-25T18:10:29Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "ConsoleLogin",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36",
    "errorMessage": "Failed authentication",
    "requestParameters": null,
    "responseElements": {
        "ConsoleLogin": "Failure"
    },
    "additionalEventData": {
        "LoginTo": "https://console.aws.amazon.com/console/home?state=hashArgs%23&isauthcode=true",
        "MobileVersion": "No",
        "MFAUsed": "No"
    },
    "eventID": "a4fbbe77-91a0-4238-804a-64314184edb6",
    "eventType": "AwsConsoleSignIn",
    "recipientAccountId": "111122223333"
}
```