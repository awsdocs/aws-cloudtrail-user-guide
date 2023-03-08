# How AWS CloudTrail works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to CloudTrail, learn what IAM features are available to use with CloudTrail\.






**IAM features you can use with AWS CloudTrail**  

| IAM feature | CloudTrail support | 
| --- | --- | 
|  [Identity\-based policies](#security_iam_service-with-iam-id-based-policies)  |    Yes  | 
|  [Resource\-based policies](#security_iam_service-with-iam-resource-based-policies)  |    Partial  | 
|  [Policy actions](#security_iam_service-with-iam-id-based-policies-actions)  |    Yes  | 
|  [Policy resources](#security_iam_service-with-iam-id-based-policies-resources)  |    Yes  | 
|  [Policy condition keys \(service\-specific\)](#security_iam_service-with-iam-id-based-policies-conditionkeys)  |    No   | 
|  [ACLs](#security_iam_service-with-iam-acls)  |    No   | 
|  [ABAC \(tags in policies\)](#security_iam_service-with-iam-tags)  |    Yes  | 
|  [Temporary credentials](#security_iam_service-with-iam-roles-tempcreds)  |    Yes  | 
|  [Principal permissions](#security_iam_service-with-iam-principal-permissions)  |    Yes  | 
|  [Service roles](#security_iam_service-with-iam-roles-service)  |    Yes  | 
|  [Service\-linked roles](#security_iam_service-with-iam-roles-service-linked)  |    Yes  | 

To get a high\-level view of how CloudTrail and other AWS services work with most IAM features, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

## Identity\-based policies for CloudTrail<a name="security_iam_service-with-iam-id-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports identity\-based policies  |    Yes  | 

Identity\-based policies are JSON permissions policy documents that you can attach to an identity, such as an IAM user, group of users, or role\. These policies control what actions users and roles can perform, on which resources, and under what conditions\. To learn how to create an identity\-based policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. You can't specify the principal in an identity\-based policy because it applies to the user or role to which it is attached\. To learn about all of the elements that you can use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Identity\-based policy examples for CloudTrail<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of CloudTrail identity\-based policies, see [Identity\-based policy examples for AWS CloudTrail](security_iam_id-based-policy-examples.md)\.

## Resource\-based policies within CloudTrail<a name="security_iam_service-with-iam-resource-based-policies"></a>


|  |  | 
| --- |--- |
|  Supports resource\-based policies  |    Partial  | 

Resource\-based policies are JSON policy documents that you attach to a resource\. Examples of resource\-based policies are IAM *role trust policies* and Amazon S3 *bucket policies*\. In services that support resource\-based policies, service administrators can use them to control access to a specific resource\. For the resource where the policy is attached, the policy defines what actions a specified principal can perform on that resource and under what conditions\. You must [specify a principal](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html) in a resource\-based policy\. Principals can include accounts, users, roles, federated users, or AWS services\.

To enable cross\-account access, you can specify an entire account or IAM entities in another account as the principal in a resource\-based policy\. Adding a cross\-account principal to a resource\-based policy is only half of establishing the trust relationship\. When the principal and the resource are in different AWS accounts, an IAM administrator in the trusted account must also grant the principal entity \(user or role\) permission to access the resource\. They grant permission by attaching an identity\-based policy to the entity\. However, if a resource\-based policy grants access to a principal in the same account, no additional identity\-based policy is required\. For more information, see [How IAM roles differ from resource\-based policies ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html)in the *IAM User Guide*\.

CloudTrail supports resource\-based policies on channels used for CloudTrail Lake integrations with event sources outside of AWS\. The resource\-based policy for the channel defines which principal entities \(accounts, users, roles, and federated users\) can call `PutAuditEvents` on the channel to deliver events to the destination event data store\. For more information about creating integrations with CloudTrail Lake, see [Create an integration with an event source outside of AWS](query-event-data-store-integration.md)\. 

**Note**  
Currently, CloudTrail Lake supports integrations in all commercial AWS Regions where CloudTrail Lake is available except for Middle East \(UAE\)\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

### Examples<a name="security_iam_service-with-iam-resource-based-policies-examples"></a>

To view examples of CloudTrail resource\-based policies, see [AWS CloudTrail resource\-based policy examples](security_iam_resource-based-policy-examples.md)\.

## Policy actions for CloudTrail<a name="security_iam_service-with-iam-id-based-policies-actions"></a>


|  |  | 
| --- |--- |
|  Supports policy actions  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.



To see a list of CloudTrail actions, see [Actions Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-actions-as-permissions) in the *Service Authorization Reference*\.

Policy actions in CloudTrail use the following prefix before the action:

```
cloudtrail
```

For example, to grant someone permission to list tags for a trail with the `ListTags` API operation, you include the `cloudtrail:ListTags` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. CloudTrail defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "cloudtrail:AddTags",
      "cloudtrail:ListTags",
      "cloudtrail:RemoveTags
```

You can specify multiple actions using wildcards \(`*`\)\. For example, to specify all actions that begin with the word `Get`, include the following action:

```
"Action": "cloudtrail:Get*"
```







## Policy resources for CloudTrail<a name="security_iam_service-with-iam-id-based-policies-resources"></a>


|  |  | 
| --- |--- |
|  Supports policy resources  |    Yes  | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```

To see a list of CloudTrail resource types and their ARNs, see [Resources Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-resources-for-iam-policies) in the *Service Authorization Reference*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-actions-as-permissions)\.





In CloudTrail, there are three resource types: trails, event data stores, and channels\. Each resource has a unique Amazon Resource Name \(ARN\) associated with it\. In a policy, you use an ARN to identify the resource that the policy applies to\. CloudTrail does not currently support other resource types, which are sometimes referred to as subresources\. 

The CloudTrail trail resource has the following ARN:

```
arn:${Partition}:cloudtrail:${Region}:${Account}:trail/{TrailName}
```

The CloudTrail event data store resource has the following ARN:

```
arn:${Partition}:cloudtrail:${Region}:${Account}:eventdatastore/{EventDataStoreId}
```

The CloudTrail channel resource has the following ARN:

```
arn:${Partition}:cloudtrail:${Region}:${Account}:channel/{ChannelId}
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\.

For example, for an AWS account with the ID *123456789012*, to specify a trail named *My\-Trail* that exists in the US East \(Ohio\) Region in your statement, use the following ARN:

```
"Resource": "arn:aws:cloudtrail:us-east-2:123456789012:trail/My-Trail"
```

To specify all trails that belong to a specific account in that AWS Region, use the wildcard \(\*\):

```
"Resource": "arn:aws:cloudtrail:us-east-2:123456789012:trail/*"
```

Some CloudTrail actions, such as those for creating resources, can't be performed on a specific resource\. In those cases, you must use the wildcard \(`*`\)\.

```
"Resource": "*"
```

Many CloudTrail API actions involve multiple resources\. For example, `CreateTrail` requires an Amazon S3 bucket for storing log files, so a user must have permissions to write to the bucket\. To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "resource1",
      "resource2"
```

## Policy condition keys for CloudTrail<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>


|  |  | 
| --- |--- |
|  Supports service\-specific policy condition keys  |    No   | 

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

CloudTrail doesn't define its own condition keys, but it supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

To see a list of CloudTrail condition keys, see [Condition Keys for AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-policy-keys) in the *Service Authorization Reference*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-actions-as-permissions)\.

## ACLs in CloudTrail<a name="security_iam_service-with-iam-acls"></a>


|  |  | 
| --- |--- |
|  Supports ACLs  |    No   | 

Access control lists \(ACLs\) control which principals \(account members, users, or roles\) have permissions to access a resource\. ACLs are similar to resource\-based policies, although they do not use the JSON policy document format\.

## ABAC with CloudTrail<a name="security_iam_service-with-iam-tags"></a>


|  |  | 
| --- |--- |
|  Supports ABAC \(tags in policies\)  |    Yes  | 

Attribute\-based access control \(ABAC\) is an authorization strategy that defines permissions based on attributes\. In AWS, these attributes are called *tags*\. You can attach tags to IAM entities \(users or roles\) and to many AWS resources\. Tagging entities and resources is the first step of ABAC\. Then you design ABAC policies to allow operations when the principal's tag matches the tag on the resource that they are trying to access\.

ABAC is helpful in environments that are growing rapidly and helps with situations where policy management becomes cumbersome\.

To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `aws:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\.

If a service supports all three condition keys for every resource type, then the value is **Yes** for the service\. If a service supports all three condition keys for only some resource types, then the value is **Partial**\.

For more information about ABAC, see [What is ABAC?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_attribute-based-access-control.html) in the *IAM User Guide*\. To view a tutorial with steps for setting up ABAC, see [Use attribute\-based access control \(ABAC\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_attribute-based-access-control.html) in the *IAM User Guide*\.

Although you can attach tags to CloudTrail resources, CloudTrail only supports controlling access to [CloudTrail Lake](cloudtrail-lake.md) event data stores based on tags\. You cannot control access to trails based on tags\.

You can attach tags to CloudTrail resources or pass tags in a request to CloudTrail\. For more information about tagging CloudTrail resources, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Creating, updating, and managing trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

## Using temporary credentials with CloudTrail<a name="security_iam_service-with-iam-roles-tempcreds"></a>


|  |  | 
| --- |--- |
|  Supports temporary credentials  |    Yes  | 

Some AWS services don't work when you sign in using temporary credentials\. For additional information, including which AWS services work with temporary credentials, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

You are using temporary credentials if you sign in to the AWS Management Console using any method except a user name and password\. For example, when you access AWS using your company's single sign\-on \(SSO\) link, that process automatically creates temporary credentials\. You also automatically create temporary credentials when you sign in to the console as a user and then switch roles\. For more information about switching roles, see [Switching to a role \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-console.html) in the *IAM User Guide*\.

You can manually create temporary credentials using the AWS CLI or AWS API\. You can then use those temporary credentials to access AWS\. AWS recommends that you dynamically generate temporary credentials instead of using long\-term access keys\. For more information, see [Temporary security credentials in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)\.

## Principal permissions for CloudTrail<a name="security_iam_service-with-iam-principal-permissions"></a>


|  |  | 
| --- |--- |
|  Supports principal permissions  |    Yes  | 

  When you use an IAM user or role to perform actions in AWS, you are considered a principal\. Policies grant permissions to a principal\. When you use some services, you might perform an action that then triggers another action in a different service\. In this case, you must have permissions to perform both actions\. To see whether an action requires additional dependent actions in a policy, see [Actions, Resources, and Condition Keys for AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html) in the *Service Authorization Reference*\. 

## Service roles for CloudTrail<a name="security_iam_service-with-iam-roles-service"></a>


|  |  | 
| --- |--- |
|  Supports service roles  |    Yes  | 

  A service role is an [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) that a service assumes to perform actions on your behalf\. An IAM administrator can create, modify, and delete a service role from within IAM\. For more information, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 

**Warning**  
Changing the permissions for a service role might break CloudTrail functionality\. Edit service roles only when CloudTrail provides guidance to do so\.

## Service\-linked roles for CloudTrail<a name="security_iam_service-with-iam-roles-service-linked"></a>


|  |  | 
| --- |--- |
|  Supports service\-linked roles  |    Yes  | 

  A service\-linked role is a type of service role that is linked to an AWS service\. The service can assume the role to perform an action on your behalf\. Service\-linked roles appear in your AWS account and are owned by the service\. An IAM administrator can view, but not edit the permissions for service\-linked roles\. 

CloudTrail supports a service\-linked role for integration with AWS Organizations\. This role is required for the creation of an organization trail or event data store\. Organization trails and event data stores log events for all AWS accounts in an organization\. For more information about creating or managing CloudTrail service\-linked roles, see [Using service\-linked roles for AWS CloudTrail](using-service-linked-roles.md)\.