# How AWS CloudTrail Works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to CloudTrail, you should understand what IAM features are available to use with CloudTrail\. To get a high\-level view of how CloudTrail and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

CloudTrail works with IAM identity\-based policies, but does not work with resource\-based policies\. For more information on the differences between identity\-based policies and resource\-based policies, see [Identity\-Based Policies and Resource\-Based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html) in the *IAM User Guide*\.

**Topics**
+ [CloudTrail Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies)
+ [CloudTrail Resource\-Based Policies](#security_iam_service-with-iam-resource-based-policies)
+ [Access Control Lists \(ACLs\)](#security_iam_service-with-iam-acls)
+ [Authorization Based on CloudTrail Tags](#security_iam_service-with-iam-tags)
+ [CloudTrail IAM Roles](#security_iam_service-with-iam-roles)

## CloudTrail Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. CloudTrail supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

The `Action` element of an IAM identity\-based policy describes the specific action or actions that will be allowed or denied by the policy\. Policy actions usually have the same name as the associated AWS API operation\. The action is used in a policy to grant permissions to perform the associated operation\. 

Policy actions in CloudTrail use the following prefix before the action: `cloudtrail:`\. For example, to grant someone permission to list tags for a trail with the `ListTags` API operation, you include the `cloudtrail:ListTags` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. CloudTrail defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "cloudtrail:AddTags",
      "cloudtrail:ListTags",
      "cloudtrail:RemoveTags
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Get`, include the following action:

```
"Action": "cloudtrail:Get*"
```



To see a list of CloudTrail actions, see [Actions Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

The `Resource` element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. You specify a resource using an ARN or using the wildcard \(\*\) to indicate that the statement applies to all resources\.



In CloudTrail, the primary resource is a trail\. Each resource has a unique Amazon Resource Name \(ARN\) associated with it\. In a policy, you use an ARN to identify the resource that the policy applies to\. CloudTrail does not currently support other resource types, which are sometimes referred to as subresources\.

The CloudTrail trail resource has the following ARN:

```
arn:${Partition}:cloudtrail:${Region}:${Account}:trail/{TrailName}
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

Some CloudTrail actions, such as those for creating resources, cannot be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

Many CloudTrail API actions involve multiple resources\. For example, `CreateTrail` requires an Amazon S3 bucket for storing log files, so an IAM user must have permissions to write to the bucket\. To specify multiple resources in a single statement, separate the ARNs with commas\. 

```
"Resource": [
      "resource1",
      "resource2"
```

To see a list of CloudTrail resource types and their ARNs, see [Resources Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-resources-for-iam-policies) in the *IAM User Guide*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-actions-as-permissions)\.

### Condition Keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can build conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM Policy Elements: Variables and Tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

CloudTrail does not define its own condition keys, but it supports using some global condition keys\. To see all AWS global condition keys, see [AWS Global Condition Context Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

To see a list of condition keys supported for CloudTrail, see [Condition Keys for AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-policy-keys) in the *IAM User Guide*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by AWS CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awscloudtrail.html#awscloudtrail-actions-as-permissions)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of CloudTrail identity\-based policies, see [AWS CloudTrail Identity\-Based Policy Examples](security_iam_id-based-policy-examples.md)\.

## CloudTrail Resource\-Based Policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

CloudTrail does not support resource\-based policies\. 

## Access Control Lists \(ACLs\)<a name="security_iam_service-with-iam-acls"></a>

Access control lists \(ACLs\) are lists of grantees that you can attach to resources\. They grant accounts permissions to access the resource to which they are attached\. While CloudTrail does not support ACLs, Amazon S3 does\. For example, you can attach ACLs to an Amazon S3 bucket resource where you store log files for one or more trails\. For more information about attaching ACLs to buckets, see [https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the Amazon Simple Storage Service Developer Guide\.

## Authorization Based on CloudTrail Tags<a name="security_iam_service-with-iam-tags"></a>

Although you can attach tags to CloudTrail resources, CloudTrail does not support controlling access based on tags\.

You can attach tags to CloudTrail resources or pass tags in a request to CloudTrail\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `cloudtrail:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. For more information about tagging CloudTrail resources, see [Creating a Trail](cloudtrail-create-a-trail-using-the-console-first-time.md) and [Creating, Updating, and Managing Trails with the AWS Command Line Interface](cloudtrail-create-and-update-a-trail-by-using-the-aws-cli.md)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Viewing CloudTrail Trails Based on Tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-trails-tags)\.

## CloudTrail IAM Roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using Temporary Credentials with CloudTrail<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS STS API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\. 

CloudTrail supports using temporary credentials\. 

### Service\-Linked Roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

CloudTrail supports a service\-linked role for integration with AWS Organizations\. This role is required for the creation of an organization trail, a trail that logs events for all AWS accounts in an organization\. For details about creating or managing CloudTrail service\-linked roles, see [Using Service\-Linked Roles for AWS CloudTrail](using-service-linked-roles.md)\.

### Service Roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

CloudTrail supports service roles\. 

### <a name="security_iam_service-with-iam-roles-choose"></a>