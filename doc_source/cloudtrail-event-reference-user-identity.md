# CloudTrail userIdentity element<a name="cloudtrail-event-reference-user-identity"></a>

AWS Identity and Access Management \(IAM\) provides different types of identities\. The `userIdentity` element contains details about the type of IAM identity that made the request, and which credentials were used\. If temporary credentials were used, the element shows how the credentials were obtained\. 

**Contents**
+ [Examples](#cloudtrail-event-reference-user-identity-examples)
+ [Fields](#cloudtrail-event-reference-user-identity-fields)
+ [Values for AWS STS APIs with SAML and web identity federation](#STS-API-SAML-WIF)
+ [AWS STS source identity](#STS-API-source-identity)

## Examples<a name="cloudtrail-event-reference-user-identity-examples"></a>

**`userIdentity` with IAM user credentials**

The following example shows the `userIdentity` element of a simple request made with the credentials of the IAM user named `Alice`\. 

```
"userIdentity": {
  "type": "IAMUser",
  "principalId": "AIDAJ45Q7YFFAREXAMPLE",
  "arn": "arn:aws:iam::123456789012:user/Alice",
  "accountId": "123456789012",
  "accessKeyId": "",
  "userName": "Alice"
}
```

**`userIdentity` with temporary security credentials**

The following example shows a `userIdentity` element for a request made with temporary security credentials obtained by assuming an IAM role\. The element contains additional details about the role that was assumed to get credentials\. 

```
"userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAIDPPEZS35WEXAMPLE:AssumedRoleSessionName",
    "arn": "arn:aws:sts::123456789012:assumed-role/RoleToBeAssumed/MySessionName",
    "accountId": "123456789012",
    "accessKeyId": "",
    "sessionContext": {
      "attributes": {
        "mfaAuthenticated": "false",
        "creationDate": "20131102T010628Z"
      },
      "sessionIssuer": {
        "type": "Role",
        "principalId": "AROAIDPPEZS35WEXAMPLE",
        "arn": "arn:aws:iam::123456789012:role/RoleToBeAssumed",
        "accountId": "123456789012",
        "userName": "RoleToBeAssumed"
      }
    }
}
```

## Fields<a name="cloudtrail-event-reference-user-identity-fields"></a>

The following fields can appear in a `userIdentity` element\.

**`type`**  
The type of the identity\. The following values are possible:  
+ `Root` – The request was made with your AWS account credentials\. If the `userIdentity` type is `Root`, and you set an alias for your account, the `userName` field contains your account alias\. For more information, see [Your AWS account ID and its alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html)\. 
+ `IAMUser` – The request was made with the credentials of an IAM user\.
+ `AssumedRole` – The request was made with temporary security credentials that were obtained with a role by making a call to the AWS Security Token Service \(AWS STS\) [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API\. This can include [roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) and [cross\-account API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/delegation-cross-acct-access.html)\. 
+ `Role` – The request was made with a persistent IAM identity that has specific permissions\. The issuer of the role sessions is always the role\. For more information about roles, see [Roles terms and concepts](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) in the *IAM User Guide*\.
+ `FederatedUser` – The request was made with temporary security credentials obtained from a call to the AWS STS [https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html) API\. The `sessionIssuer` element indicates if the API was called with root or IAM user credentials\.

  For more information about temporary security credentials, see [Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) in the *IAM User Guide*\.
+ `Directory` – The request was made to a directory service, and the type is unknown\. Directory services include the following: Amazon WorkDocs and Amazon QuickSight\.
+ `AWSAccount` – The request was made by another AWS account
+ `AWSService` – The request was made by an AWS account that belongs to an AWS service\. For example, AWS Elastic Beanstalk assumes an IAM role in your account to call other AWS services on your behalf\.
+ `Unknown` – The request was made with an identity type that CloudTrail can't determine\.
**Optional:** False  
`AWSAccount` and `AWSService` appear for `type` in your logs when there is cross\-account access using an IAM role that you own\.  

**Example: Cross\-account access initiated by another AWS account**

1. You own an IAM role in your account\. 

1. Another AWS account switches to that role to assume the role for your account\.

1. Because you own the IAM role, you receive a log that shows the other account assumed the role\. The `type` is `AWSAccount`\. For an example log entry, see [AWS STS API event in CloudTrail log file](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html#stscloudtrailexample)\. 

**Example: Cross\-account access initiated by an AWS service**

1. You own an IAM role in your account\. 

1. An AWS account owned by an AWS service assumes that role\.

1. Because you own the IAM role, you receive a log that shows the AWS service assumed the role\. The `type` is `AWSService`\.

**`userName`**  
The friendly name of the identity that made the call\. The value that appears in `userName` is based on the value in `type`\. The following table shows the relationship between `type` and `userName`:      
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)
**Optional:** True  
The `userName` field contains the string `HIDDEN_DUE_TO_SECURITY_REASONS` when the recorded event is a console sign\-in failure caused by incorrect user name input\. CloudTrail does not record the contents in this case because the text could contain sensitive information, as in the following examples:  
+ A user accidentally types a password in the user name field\.
+ A user clicks the link for one AWS account's sign\-in page, but then types the account number for a different one\.
+ A user accidentally types the account name of a personal email account, a bank sign\-in identifier, or some other private ID\. 

**`principalId`**  
A unique identifier for the entity that made the call\. For requests made with temporary security credentials, this value includes the session name that is passed to the `AssumeRole`, `AssumeRoleWithWebIdentity`, or `GetFederationToken` API call\.  
**Optional:** True

**`arn`**  
The Amazon Resource Name \(ARN\) of the principal that made the call\. The last section of the arn contains the user or role that made the call\.  
**Optional:** True

**`accountId`**  
The account that owns the entity that granted permissions for the request\. If the request was made with temporary security credentials, this is the account that owns the IAM user or role used to obtain credentials\.   
**Optional:** True

**`accessKeyId`**  
The access key ID that was used to sign the request\. If the request was made with temporary security credentials, this is the access key ID of the temporary credentials\. For security reasons, `accessKeyId` might not be present, or might be displayed as an empty string\.  
**Optional:** True

**`sessionContext`**  
If the request was made with temporary security credentials, `sessionContext` provides information about the session created for those credentials\. You create a session when you call any API that returns temporary credentials\. Users also create sessions when they work in the console and make requests with APIs that include [multi\-factor authentication](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html)\. This element has the following attributes:  
+ `creationDate` – The date and time when the temporary security credentials were issued\. Represented in ISO 8601 basic notation\. 
+ `mfaAuthenticated` – The value is `true` if the root user or IAM user who used their credentials for the request also authenticated with an MFA device; otherwise, `false`\.
+ `sourceIdentity` – See [AWS STS source identity](#STS-API-source-identity) in this topic\. The `sourceIdentity` field occurs in events when users assume an IAM role to perform an action\. `sourceIdentity` identifies the original user identity making the request, whether that user's identity is an IAM user, an IAM role, a user authenticated through SAML\-based federation, or a user authenticated through OpenID Connect \(OIDC\)\-compliant web identity federation\. For more information about configuring AWS STS to collect source identity information, see [Monitor and control actions taken with assumed roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_monitor.html) in the *IAM User Guide*\.
+ `ec2RoleDelivery` – The value is `1.0` if the credentials were provided by Amazon EC2 Instance Metadata Service Version 1 \(IMDSv1\)\. The value is `2.0` if the credentials were provided using the new IMDS scheme\.

  AWS credentials provided by the Amazon EC2 Instance Metadata Service \(IMDS\) include an ec2:RoleDelivery IAM context key\. This context key makes it easy to enforce use of the new scheme on a service\-by\-service or resource\-by\-resource basis by using the context key as a condition in IAM policies, resource policies, or AWS Organizations service control policies\. For more information, see [Instance metadata and user data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Optional:** True

**`invokedBy`**  
The name of the AWS service that made the request, such as Amazon EC2 Auto Scaling or AWS Elastic Beanstalk\.  
**Optional:** True

**`sessionIssuer`**  
If a user make a request with temporary security credentials, `sessionIssuer` provides information about how the user obtained credentials\. For example, if the they obtained temporary security credentials by assuming a role, this element provides information about the assumed role\. If they obtained credentials with root or IAM user credentials to call AWS STS `GetFederationToken`, the element provides information about the root account or IAM user\. This element has the following attributes:  
+ `type` – The source of the temporary security credentials, such as `Root`, `IAMUser`, or `Role`\. 
+ `userName` – The friendly name of the user or role that issued the session\. The value that appears depends on the `sessionIssuer` identity `type`\. The following table shows the relationship between `sessionIssuer type` and `userName`:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)
+ `principalId` – The internal ID of the entity used to get credentials\.
+ `arn` – The ARN of the source \(account, IAM user, or role\) that was used to get temporary security credentials\.
+ `accountId` – The account that owns the entity that was used to get credentials\.
**Optional:** True

**`webIdFederationData`**  
If the request was made with temporary security credentials obtained by [web identity federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html), `webIdFederationData` lists information about the identity provider\. This element has the following attributes:  
+ `federatedProvider` – The principal name of the identity provider \(for example, `www.amazon.com` for Login with Amazon or `accounts.google.com` for Google\)\. 
+ `attributes` – The application ID and user ID as reported by the provider \(for example, `www.amazon.com:app_id` and `www.amazon.com:user_id` for Login with Amazon\)\.
**Optional:** True

## Values for AWS STS APIs with SAML and web identity federation<a name="STS-API-SAML-WIF"></a>

AWS CloudTrail supports logging AWS Security Token Service \(AWS STS\) API calls made with Security Assertion Markup Language \(SAML\) and web identity federation\. When a user makes a call to the [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html) and [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) APIs, CloudTrail records the call and delivers the event to your Amazon S3 bucket\.

The `userIdentity` element for these APIs contains the following values\.

**`type`**  
The identity type\.  
+ `SAMLUser` – The request was made with SAML assertion\.
+ `WebIdentityUser` – The request was made by a web identity federation provider\.

**`principalId`**  
A unique identifier for the entity that made the call\.  
+ For `SAMLUser`, this is a combination of the `saml:namequalifier` and `saml:sub` keys\. 
+ For `WebIdentityUser`, this is a combination of the issuer, application ID, and user ID\.

**`userName`**  
The name of the identity that made the call\.  
+ For `SAMLUser`, this is the `saml:sub` key\.
+ For `WebIdentityUser`, this is the user ID\.

**`identityProvider`**  
The principal name of the external identity provider\. This field appears only for `SAMLUser` or `WebIdentityUser` types\.  
+ For `SAMLUser`, this is the `saml:namequalifier` key for the SAML assertion\. 
+ For `WebIdentityUser`, this is the issuer name of the web identity federation provider\. This can be a provider that you configured, such as the following:
  + `cognito-identity.amazon.com` for Amazon Cognito
  + `www.amazon.com` for Login with Amazon
  + `accounts.google.com` for Google
  + `graph.facebook.com` for Facebook

The following is an example `userIdentity` element for the `AssumeRoleWithWebIdentity` action\.

```
"userIdentity": {
    "type": "WebIdentityUser",
    "principalId": "accounts.google.com:application-id.apps.googleusercontent.com:user-id",
    "userName": "user-id",
    "identityProvider": "accounts.google.com"
  }
```

For example logs of how the `userIdentity` element appears for `SAMLUser` and `WebIdentityUser` types, see [ Logging IAM and AWS STS API calls with AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html)\.

## AWS STS source identity<a name="STS-API-source-identity"></a>

An IAM administrator can configure AWS Security Token Service to require that users specify their identity when they use temporary credentials to assume roles\. The `sourceIdentity` ﬁeld occurs in events when users assume an IAM role or perform any actions with the assumed role\.

The `sourceIdentity` field identifies the original user identity making the request, whether that user's identity is an IAM user, an IAM role, a user authenticated by using SAML\-based federation, or a user authenticated by using OpenID Connect \(OIDC\)\-compliant web identity federation\. After the IAM administrator configures AWS STS, CloudTrail logs `sourceIdentity` information in the following events and locations within the event record:
+ The AWS STS `AssumeRole`, `AssumeRoleWithSAML`, or `AssumeRoleWithWebIdentity` calls that a user identity makes when it assumes a role\. `sourceIdentity` is found in the `requestParameters` block of the AWS STS calls\.
+ The AWS STS `AssumeRole`, `AssumeRoleWithSAML`, or `AssumeRoleWithWebIdentity` calls that a user identity makes if it uses a role to assume another role, known as [role chaining](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-role-chaining)\. `sourceIdentity` is found in the `requestParameters` block of the AWS STS calls\.
+ The AWS service API calls that the user identity makes while assuming a role and using the temporary credentials assigned by AWS STS\. In service API events, `sourceIdentity` is found in the `sessionContext` block\. For example, if a user identity creates a new S3 bucket, `sourceIdentity` occurs in the `sessionContext` block of the `CreateBucket` event\.

For more information about how to configure AWS STS to collect source identity information, see [Monitor and control actions taken with assumed roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_monitor.html) in the *IAM User Guide*\. For more information about AWS STS events that are logged to CloudTrail, see [Logging IAM and AWS STS API calls with AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html) in the *IAM User Guide*\.

The following are example snippets of events that show the `sourceIdentity` field\.

**Example `requestParameters` section**

In the following example event snippet, a user makes an AWS STS `AssumeRole` request, and sets a source identity, represented here by `source-identity-value-set`\. The user assumes a role represented by the role ARN `arn:aws:iam::123456789012:role/Assumed_Role`\. The `sourceIdentity` field is in the `requestParameters` block of the event\.

```
"eventVersion": "1.05",
    "userIdentity": {
        "type": "AWSAccount",
        "principalId": "AIDAJ45Q7YFFAREXAMPLE",
        "accountId": "123456789012"
    },
    "eventTime": "2020-04-02T18:20:53Z",
    "eventSource": "sts.amazonaws.com",
    "eventName": "AssumeRole",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "203.0.113.64",
    "userAgent": "aws-cli/1.16.96 Python/3.6.0 Windows/10 botocore/1.12.86",
    "requestParameters": {
        "roleArn": "arn:aws:iam::123456789012:role/Assumed_Role",
        "roleSessionName": "Test1",
        "sourceIdentity": "source-identity-value-set",
    },
```

**Example `responseElements` section**

In the following example event snippet, a user makes an AWS STS `AssumeRole` request to assume a role named `Developer_Role`, and sets a source identity, `Admin`\. The user assumes a role represented by the role ARN `arn:aws:iam::111122223333:role/Developer_Role`\. The `sourceIdentity` field is shown in both the `requestParameters` and `responseElements` blocks of the event\. The temporary credentials used to assume the role, the session token string, and the assumed role ID, session name, and session ARN are shown in the `responseElements` block, along with the source identity\.

```
    "requestParameters": {
        "roleArn": "arn:aws:iam::111122223333:role/Developer_Role",
        "roleSessionName": "Session_Name",
        "sourceIdentity": "Admin"
    },
    "responseElements": {
        "credentials": {
            "accessKeyId": "ASIAIOSFODNN7EXAMPLE",
            "expiration": "Jan 22, 2021 12:46:28 AM",
            "sessionToken": "XXYYaz...
                             EXAMPLE_SESSION_TOKEN
                             XXyYaZAz"
        },
        "assumedRoleUser": {
            "assumedRoleId": "AROACKCEVSQ6C2EXAMPLE:Session_Name",
            "arn": "arn:aws:sts::111122223333:assumed-role/Developer_Role/Session_Name"
        },
        "sourceIdentity": "Admin"
    }
...
```

**Example `sessionContext` section**

In the following example event snippet, a user is assuming a role named `DevRole` to call an AWS service API\. The user sets a source identity, represented here by *source\-identity\-value\-set*\. The `sourceIdentity` field is in the `sessionContext` block, within the `userIdentity` block of the event\.

```
{
  "eventVersion": "1.08",
  "userIdentity": {
    "type": "AssumedRole",
    "principalId": "AROAJ45Q7YFFAREXAMPLE: Dev1",
    "arn": "arn: aws: sts: : 123456789012: assumed-role/DevRole/Dev1",
    "accountId": "123456789012",
    "accessKeyId": "ASIAIOSFODNN7EXAMPLE",
    "sessionContext": {
      "sessionIssuer": {
        "type": "Role",
        "principalId": "AROAJ45Q7YFFAREXAMPLE",
        "arn": "arn: aws: iam: : 123456789012: role/DevRole",
        "accountId": "123456789012",
        "userName": "DevRole"
      },
      "webIdFederationData": {},
      "attributes": {
        "mfaAuthenticated": "false",
        "creationDate": "2021-02-21T23: 46: 28Z"
      },
      "sourceIdentity": "source-identity-value-set"
    }
  }
}
```