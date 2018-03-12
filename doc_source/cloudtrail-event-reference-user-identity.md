# CloudTrail userIdentity Element<a name="cloudtrail-event-reference-user-identity"></a>

AWS Identity and Access Management \(IAM\) provides different types of identities\. The `userIdentity` element contains details about the type of IAM identity that made the request, and which credentials were used\. If temporary credentials were used, the element shows how the credentials were obtained\. 



## Examples<a name="cloudtrail-event-reference-user-identity-examples"></a>

**`userIdentity` with IAM user credentials**

The following example shows the `userIdentity` element of a simple request made with the credentials of the IAM user named `Alice`\. 

```
"userIdentity": {
  "type": "IAMUser",
  "principalId": "AIDAJ45Q7YFFAREXAMPLE",
  "arn": "arn:aws:iam::123456789012:user/Alice",
  "accountId": "123456789012",
  "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
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
    "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
    "sessionContext": {
      "attributes": {
        "creationDate": "20131102T010628Z",
        "mfaAuthenticated": "false"
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

+ `Root` – The request was made with your AWS account credentials\. If the `userIdentity` type is `Root` and you set an alias for your account, the `userName` field contains your account alias\. For more information, see [Your AWS Account ID and Its Alias](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccountAlias.html)\. 

+ `IAMUser` – The request was made with the credentials of an IAM user\.

+ `AssumedRole` – The request was made with temporary security credentials that were obtained with a role via a call to the AWS Security Token Service \(AWS STS\) [http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) API\. This can include [roles for Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) and [cross\-account API access](http://docs.aws.amazon.com/IAM/latest/UserGuide/delegation-cross-acct-access.html)\. 

+ `FederatedUser` – The request was made with temporary security credentials that were obtained via a call to the AWS STS [http://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html](http://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html) API\. The `sessionIssuer` element indicates if the API was called with root or IAM user credentials\. 

  For more information about temporary security credentials, see [Temporary Security Credentials](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) in the *IAM User Guide*\.

+ `AWSAccount` – The request was made by another AWS account\.

+ `AWSService` – The request was made by an AWS account that belongs to an AWS service\. For example, AWS Elastic Beanstalk assumes an IAM role in your account to call other AWS services on your behalf\.
`AWSAccount` and `AWSService` appear for `type` in your logs when there is cross\-account access using an IAM role that you own\.  

**Example: Cross\-account access initiated by another AWS account**

1. You own an IAM role in your account\. 

1. Another AWS account switches to that role to assume the role for your account\.

1. Because you own the IAM role, you receive a log that shows the other account assumed the role\. The `type` is `AWSAccount`\. For an example log entry, see [AWS STS API Event in CloudTrail Log File](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html#stscloudtrailexample)\. 

**Example: Cross\-account access initiated by an AWS service**

1. You own an IAM role in your account\. 

1. An AWS account owned by an AWS service assumes that role\.

1. Because you own the IAM role, you receive a log that shows the AWS service assumed the role\. The `type` is `AWSService`\.

**`userName`**  
The friendly name of the identity that made the call\. The value that appears in `userName` is based on the value in `type`\. The following table shows the relationship between `type` and `userName`:     
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)
The `userName` field contains the string `HIDDEN_DUE_TO_SECURITY_REASONS` when the recorded event is a console sign\-in failure caused by incorrect user name input\. CloudTrail does not record the contents in this case because the text could contain sensitive information, as in the following examples:  

+ A user accidentally types a password in the user name field\.

+ A user clicks the link for one AWS account's sign\-in page, but then types the account number for a different one\.

+ A user accidentally types the account name of a personal email account, a bank sign\-in identifier, or some other private ID\. 

**`principalId`**  
A unique identifier for the entity that made the call\. For requests made with temporary security credentials, this value includes the session name that is passed to the `AssumeRole`, `AssumeRoleWithWebIdentity`, or `GetFederationToken` API call\.

**`arn`**  
The Amazon Resource Name \(ARN\) of the principal that made the call\. The last section of the arn contains the user or role that made the call\.

**`accountId`**  
The account that owns the entity that granted permissions for the request\. If the request was made with temporary security credentials, this is the account that owns the IAM user or role that was used to obtain credentials\. 

**`accessKeyId`**  
The access key ID that was used to sign the request\. If the request was made with temporary security credentials, this is the access key ID of the temporary credentials\. 

**`sessionContext`**  
If the request was made with temporary security credentials, an element that provides information about the session that was created for those credentials\. Sessions are created when any API is called that returns temporary credentials\. Sessions are also created when users work in the console and when users make a request with APIs that include [multi\-factor authentication](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html)\. Attributes for this element are:  

+ `creationDate` – The date and time when the temporary security credentials were issued\. Represented in ISO 8601 basic notation\. 

+ `mfaAuthenticated` – The value is `true` if the root user or IAM user whose credentials were used for the request also was authenticated with an MFA device; otherwise, `false`\.

**`invokedBy`**  
The name of the AWS service that made the request, such as Amazon EC2 Auto Scaling or AWS Elastic Beanstalk\. 

**`sessionIssuer`**  
If the request was made with temporary security credentials, an element that provides information about how the credentials were obtained\. For example, if the temporary security credentials were obtained by assuming a role, this element provides information about the assumed role\. If the credentials were obtained with root or IAM user credentials to call AWS STS `GetFederationToken`, the element provides information about the root account or IAM user\. Attributes for this element are:  

+ `type` – The source of the temporary security credentials, such as `Root`, `IAMUser`, or `Role`\. 

+ ` userName` – The friendly name of the user or role that issued the session\. The value that appears depends on the `sessionIssuer` identity `type`\. The following table shows the relationship between `sessionIssuer type` and `userName`:   
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)

+ `principalId` – The internal ID of the entity that was used to get credentials\.

+ `arn` – The ARN of the source \(account, IAM user, or role\) that was used to get temporary security credentials\.

+ `accountId` – The account that owns the entity that was used to get credentials\.

**`webIdFederationData`**  
If the request was made with temporary security credentials obtained by [web identity federation](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html), an element that lists information about the identity provider\. Attributes for this element are:  

+ `federatedProvider` – The principal name of the identity provider \(for example, `www.amazon.com` for Login with Amazon or `accounts.google.com` for Google\)\. 

+ `attributes` – The application ID and user ID as reported by the provider \(for example, `www.amazon.com:app_id` and `www.amazon.com:user_id` for Login with Amazon\)\. For more information, see [Available Keys for Web Identity Federation](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#condition-keys-wif) in the *IAM User Guide*\. 

## Values for AWS STS APIs with SAML and Web Identity Federation<a name="STS-API-SAML-WIF"></a>

AWS CloudTrail supports logging AWS Security Token Service \(AWS STS\) API calls made with Security Assertion Markup Language \(SAML\) and web identity federation\. When a call is made to the [http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html) and [http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithWebIdentity.html) APIs, CloudTrail records the call and delivers the event to your Amazon S3 bucket\. 

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

+ For `SAMLUser`, this is the `saml:sub` key\. See [Available Keys for SAML\-Based Federation](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#condition-keys-saml)\.

+ For `WebIdentityUser`, this is the user ID\. See [Available Keys for Web Identity Federation](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#condition-keys-wif)\.

**`identityProvider`**  
The principal name of the external identity provider\. This field appears only for `SAMLUser` or `WebIdentityUser` types\.   

+ For `SAMLUser`, this is the `saml:namequalifier` key for the SAML assertion\. 

+  For `WebIdentityUser`, this is the issuer name of the web identity federation provider\. This can be a provider that you configured, such as the following:

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

For example logs of how the `userIdentity` element appears for `SAMLUser` and `WebIdentityUser` types, see [Logging IAM Events with AWS CloudTrail](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html)\.