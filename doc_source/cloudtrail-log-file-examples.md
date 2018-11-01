# CloudTrail Log File Examples<a name="cloudtrail-log-file-examples"></a>

CloudTrail monitors events for your account\. If you create a trail, it delivers those events as log files to your Amazon S3 bucket\. See the following to learn more about log files\.

**Topics**
+ [CloudTrail Log File Name Format](#cloudtrail-log-filename-format)
+ [Log File Examples](#cloudtrail-log-file-examples-section)

## CloudTrail Log File Name Format<a name="cloudtrail-log-filename-format"></a>

CloudTrail uses the following file name format for the log file objects that it delivers to your Amazon S3 bucket:

```
AccountID_CloudTrail_RegionName_YYYYMMDDTHHmmZ_UniqueString.FileNameFormat 
```
+ The `YYYY`, `MM`, `DD`, `HH`, and `mm` are the digits of the year, month, day, hour, and minute when the log file was delivered\. Hours are in 24\-hour format\. The `Z` indicates that the time is in UTC\. 
**Note**  
A log file delivered at a specific time can contain records written at any point before that time\.
+ The 16\-character `UniqueString` component of the log file name is there to prevent overwriting of files\. It has no meaning, and log processing software should ignore it\. 
+ `FileNameFormat` is the encoding of the file\. Currently, this is `json.gz`, which is a JSON text file in compressed gzip format\.

 **Example CloudTrail Log File Name**

```
111122223333_CloudTrail_us-east-2_20150801T0210Z_Mu0KsOhtH1ar15ZZ.json.gz 
```

## Log File Examples<a name="cloudtrail-log-file-examples-section"></a>

A log file contains one or more records\. The following examples are snippets of logs that show the records for an action that started the creation of a log file\. 

**Contents**
+ [Amazon EC2 Log Examples](#cloudtrail-log-file-examples-ec2)
+ [IAM Log Examples](#cloudtrail-log-file-examples-iam)
+ [Error Code and Message Log Example](#error-code-and-error-message)

### Amazon EC2 Log Examples<a name="cloudtrail-log-file-examples-ec2"></a>

Amazon Elastic Compute Cloud \(Amazon EC2\) provides resizeable computing capacity in the AWS Cloud\. You can launch virtual servers, configure security and networking, and manage storage\. Amazon EC2 can also scale up or down quickly to handle changes in requirements or spikes in popularity, thereby reducing your need to forecast server traffic\. For more information, see the [Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)\.

The following example shows that an IAM user named Alice used the AWS CLI to call the Amazon EC2 `StartInstances` action by using the `ec2-start-instances` command for instance `i-ebeaf9e2`\. 

```
{"Records": [{
    "eventVersion": "1.0",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Alice",
        "accessKeyId": "EXAMPLE_KEY_ID",
        "accountId": "123456789012",
        "userName": "Alice"
    },
    "eventTime": "2014-03-06T21:22:54Z",
    "eventSource": "ec2.amazonaws.com",
    "eventName": "StartInstances",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "205.251.233.176",
    "userAgent": "ec2-api-tools 1.6.12.2",
    "requestParameters": {"instancesSet": {"items": [{"instanceId": "i-ebeaf9e2"}]}},
    "responseElements": {"instancesSet": {"items": [{
        "instanceId": "i-ebeaf9e2",
        "currentState": {
            "code": 0,
            "name": "pending"
        },
        "previousState": {
            "code": 80,
            "name": "stopped"
        }
    }]}}
}]}
```

The following example shows that an IAM user named Alice used the AWS CLI to call the Amazon EC2 `StopInstances`action by using the `ec2-stop-instances`\. 

```
{"Records": [{
    "eventVersion": "1.0",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Alice",
        "accountId": "123456789012",
        "accessKeyId": "EXAMPLE_KEY_ID",
        "userName": "Alice"
    },
    "eventTime": "2014-03-06T21:01:59Z",
    "eventSource": "ec2.amazonaws.com",
    "eventName": "StopInstances",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "205.251.233.176",
    "userAgent": "ec2-api-tools 1.6.12.2",
    "requestParameters": {
        "instancesSet": {"items": [{"instanceId": "i-ebeaf9e2"}]},
        "force": false
    },
    "responseElements": {"instancesSet": {"items": [{
        "instanceId": "i-ebeaf9e2",
        "currentState": {
            "code": 64,
            "name": "stopping"
        },
        "previousState": {
            "code": 16,
            "name": "running"
        }
    }]}}
}]}
```

The following example shows that the Amazon EC2 console backend called the `CreateKeyPair` action in response to requests initiated by the IAM user Alice\. Note that the `responseElements` contain a hash of the key pair and that the key material has been removed by AWS\. 

```
{"Records": [{
    "eventVersion": "1.0",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Alice",
        "accountId": "123456789012",
        "accessKeyId": "EXAMPLE_KEY_ID",
        "userName": "Alice",
        "sessionContext": {"attributes": {
            "mfaAuthenticated": "false",
            "creationDate": "2014-03-06T15:15:06Z"
        }}
    },
    "eventTime": "2014-03-06T17:10:34Z",
    "eventSource": "ec2.amazonaws.com",
    "eventName": "CreateKeyPair",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "72.21.198.64",
    "userAgent": "EC2ConsoleBackend, aws-sdk-java/Linux/x.xx.fleetxen Java_HotSpot(TM)_64-Bit_Server_VM/xx",
    "requestParameters": {"keyName": "mykeypair"},
    "responseElements": {
        "keyName": "mykeypair",
        "keyFingerprint": "30:1d:46:d0:5b:ad:7e:1b:b6:70:62:8b:ff:38:b5:e9:ab:5d:b8:21",
        "keyMaterial": "\u003csensitiveDataRemoved\u003e"
    }
}]}
```

### IAM Log Examples<a name="cloudtrail-log-file-examples-iam"></a>

AWS Identity and Access Management \(IAM\) is a web service that enables AWS customers to manage users and user permissions\. With IAM, you can manage users, security credentials such as access keys, and permissions that control which AWS resources users can access\. For more information, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

The following example shows that the IAM user Alice used the AWS CLI to call the `CreateUser` action to create a new user named Bob\.

```
{"Records": [{
    "eventVersion": "1.0",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Alice",
        "accountId": "123456789012",
        "accessKeyId": "EXAMPLE_KEY_ID",
        "userName": "Alice"
    },
    "eventTime": "2014-03-24T21:11:59Z",
    "eventSource": "iam.amazonaws.com",
    "eventName": "CreateUser",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "127.0.0.1",
    "userAgent": "aws-cli/1.3.2 Python/2.7.5 Windows/7",
    "requestParameters": {"userName": "Bob"},
    "responseElements": {"user": {
        "createDate": "Mar 24, 2014 9:11:59 PM",
        "userName": "Bob",
        "arn": "arn:aws:iam::123456789012:user/Bob",
        "path": "/",
        "userId": "EXAMPLEUSERID"
    }}
}]}
```

The following example shows that the IAM user Alice used the AWS Management Console to call the `AddUserToGroup` action to add Bob to the administrator group\.

```
{"Records": [{
    "eventVersion": "1.0",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Alice",
        "accountId": "123456789012",
        "accessKeyId": "EXAMPLE_KEY_ID",
        "userName": "Alice",
        "sessionContext": {"attributes": {
            "mfaAuthenticated": "false",
            "creationDate": "2014-03-25T18:45:11Z"
        }}
    },
    "eventTime": "2014-03-25T21:08:14Z",
    "eventSource": "iam.amazonaws.com",
    "eventName": "AddUserToGroup",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "127.0.0.1",
    "userAgent": "AWSConsole",
    "requestParameters": {
        "userName": "Bob",
        "groupName": "admin"
    },
    "responseElements": null
}]}
```

The following example shows that the IAM user Alice used the AWS CLI to call the `CreateRole` action to create a new IAM role\.

```
{
    "Records": [{
        "eventVersion": "1.0",
        "userIdentity": {
            "type": "IAMUser",
            "principalId": "EX_PRINCIPAL_ID",
            "arn": "arn:aws:iam::123456789012:user/Alice",
            "accountId": "123456789012",
            "accessKeyId": "EXAMPLE_KEY_ID",
            "userName": "Alice"
        },
        "eventTime": "2014-03-25T20:17:37Z",
        "eventSource": "iam.amazonaws.com",
        "eventName": "CreateRole",
        "awsRegion": "us-east-2",
        "sourceIPAddress": "127.0.0.1",
        "userAgent": "aws-cli/1.3.2 Python/2.7.5 Windows/7",
        "requestParameters": {
            "assumeRolePolicyDocument": "{\n  \"Version\": \"2012-10-17\",\n  \"Statement\": [\n    {\n      \"Sid\": \"\",
            \n\"Effect\": \"Allow\",\n      \"Principal\": {\n        \"AWS\": \"arn:aws:iam::210987654321:root\"\n      },\n      \"Action\": \"sts:AssumeRole\"\n    }\n  ]\n}",
            "roleName": "TestRole"
        },
        "responseElements": {
            "role": {
                "assumeRolePolicyDocument": "%7B%0A%20%20%22Version%22%3A%20%222012-10-17%22%2C%0A%20%20%22Statement%22%3A%20%5B%0A%20%20%20%20%7B%0A%20%20%20%20%20%20%22Sid%22%3A%20%22%22%2C%0A%20%20%20%20%20%20%22Effect%22%3A%20%22Allow%22%2C%0A%20%20%20%20%20%20%22Principal%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22AWS%22%3A%20%22arn%3Aaws%3Aiam%3A%3A803981987763%3Aroot%22%0A%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%22Action%22%3A%20%22sts%3AAssumeRole%22%0A%20%20%20%20%7D%0A%20%20%5D%0A%7D",
                "roleName": "TestRole",
                "roleId": "AROAIUU2EOWSWPGX2UJUO",
                "arn": "arn:aws:iam::123456789012:role/TestRole",
                "createDate": "Mar 25, 2014 8:17:37 PM",
                "path": "/"
            }
        }
    }]
}
```

### Error Code and Message Log Example<a name="error-code-and-error-message"></a>

The following example shows that the IAM user Alice used the AWS CLI to call the `UpdateTrail` action to update a trail named `myTrail2`, but the trail name was not found\. The log shows this error in the `errorCode` and `errorMessage` elements\. 

```
{"Records": [{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "EX_PRINCIPAL_ID",
        "arn": "arn:aws:iam::123456789012:user/Alice",
        "accountId": "123456789012",
        "accessKeyId": "EXAMPLE_KEY_ID",
        "userName": "Alice"
    },
    "eventTime": "2016-07-14T19:15:45Z",
    "eventSource": "cloudtrail.amazonaws.com",
    "eventName": "UpdateTrail",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "205.251.233.182",
    "userAgent": "aws-cli/1.10.32 Python/2.7.9 Windows/7 botocore/1.4.22",
    "errorCode": "TrailNotFoundException",
    "errorMessage": "Unknown trail: myTrail2 for the user: 123456789012",
    "requestParameters": {"name": "myTrail2"},
    "responseElements": null,
    "requestID": "5d40662a-49f7-11e6-97e4-d9cb6ff7d6a3",
    "eventID": "b7d4398e-b2f0-4faa-9c76-e2d316a8d67f",
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}]}
```