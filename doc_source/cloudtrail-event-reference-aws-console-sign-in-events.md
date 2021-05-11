# AWS console sign\-in events<a name="cloudtrail-event-reference-aws-console-sign-in-events"></a>

CloudTrail logs attempts to sign into the AWS Management Console, the AWS Discussion Forums, and the AWS Support Center\. All IAM user and root user sign\-in events, as well as all federated user sign\-in events, generate records in CloudTrail log files\. AWS console sign\-in events are global service events\.

**Topics**
+ [Example records for IAM users](#cloudtrail-event-reference-aws-console-sign-in-events-iam-user)
+ [Example event records for root users](#cloudtrail-event-reference-aws-console-sign-in-events-root)

## Example records for IAM users<a name="cloudtrail-event-reference-aws-console-sign-in-events-iam-user"></a>

The following examples show event records for several IAM user sign\-in scenarios\.

**Topics**
+ [IAM user, successful sign\-in](#cloudtrail-aws-console-sign-in-events-iam-user-success)
+ [IAM user, signing in with MFA](#cloudtrail-aws-console-sign-in-events-iam-user-mfa)
+ [IAM user, unsuccessful sign\-in](#cloudtrail-aws-console-sign-in-events-iam-user-failure)
+ [IAM user, sign\-in process checks for MFA](#cloudtrail-aws-console-sign-in-requires-mfa)

### IAM user, successful sign\-in<a name="cloudtrail-aws-console-sign-in-events-iam-user-success"></a>

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

### IAM user, signing in with MFA<a name="cloudtrail-aws-console-sign-in-events-iam-user-mfa"></a>

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

### IAM user, unsuccessful sign\-in<a name="cloudtrail-aws-console-sign-in-events-iam-user-failure"></a>

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

### IAM user, sign\-in process checks for MFA<a name="cloudtrail-aws-console-sign-in-requires-mfa"></a>

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

## Example event records for root users<a name="cloudtrail-event-reference-aws-console-sign-in-events-root"></a>

The following examples show event records for several `root` user sign\-in scenarios\.

**Topics**
+ [Root user, successful sign\-in](#cloudtrail-signin-root)
+ [Root user, unsuccessful sign\-in](#cloudtrail-unsuccessful-signin-root)
+ [Root user, MFA changed](#cloudtrail-signin-mfa-changed-root)
+ [Root user, password changed](#cloudtrail-root-password-changed)

### Root user, successful sign\-in<a name="cloudtrail-signin-root"></a>

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

### Root user, unsuccessful sign\-in<a name="cloudtrail-unsuccessful-signin-root"></a>

The following shows an unsuccessful sign\-in event for a root user not using MFA\.

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

### Root user, MFA changed<a name="cloudtrail-signin-mfa-changed-root"></a>

The following shows an example event for a root user changing multi\-factor authentication \(MFA\) settings\.

```
{
    "eventVersion": "1.05",
    "userIdentity": {
        "type": "Root",
        "principalId": "AIDACKCEVSQ6C2EXAMPLE",
        "arn": "arn:aws:iam::111122223333:root",
        "accountId": "111122223333",
        "accessKeyId": "EXAMPLE",
        "sessionContext": {
            "sessionIssuer": {},
            "webIdFederationData": {},
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2020-10-13T21:05:40Z"
            }
        }
    },
    "eventTime": "2020-10-13T21:19:09Z",
    "eventSource": "iam.amazonaws.com",
    "eventName": "EnableMFADevice",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Coral/Netty4",
    "requestParameters": {
        "userName": "AWS ROOT USER",
        "serialNumber": "arn:aws:iam::111122223333:mfa/root-account-mfa-device"
    },
    "responseElements": null,
    "requestID": "EXAMPLE4-2cf7-4a44-af00-f61f0EXAMPLE",
    "eventID": "EXAMPLEb-7dae-48cb-895f-20e86EXAMPLE",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "recipientAccountId": "111122223333"
}
```

### Root user, password changed<a name="cloudtrail-root-password-changed"></a>

The following shows an example event for a root user changing their password\.

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
    "eventTime": "2020-10-13T21:47:13Z",
    "eventSource": "signin.amazonaws.com",
    "eventName": "PasswordUpdated",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "192.0.2.0",
    "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:78.0) Gecko/20100101 Firefox/78.0",
    "requestParameters": null,
    "responseElements": {
        "PasswordUpdated": "Success"
    },
    "eventID": "EXAMPLEd-244c-4044-abbf-21c64EXAMPLE",
    "eventType": "AwsConsoleSignIn",
    "recipientAccountId": "111122223333"
}
```