# Using service\-linked roles for AWS CloudTrail<a name="using-service-linked-roles"></a>

AWS CloudTrail uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to CloudTrail\. Service\-linked roles are predefined by CloudTrail and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up CloudTrail easier because you don’t have to manually add the necessary permissions\. CloudTrail defines the permissions of its service\-linked roles, and unless defined otherwise, only CloudTrail can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for CloudTrail<a name="slr-permissions"></a>

CloudTrail uses the service\-linked role named **AWSServiceRoleForCloudTrail** – This service linked role is used for supporting the organization trail feature\.

The AWSServiceRoleForCloudTrail service\-linked role trusts the following services to assume the role:
+ `cloudtrail.amazonaws.com`

This role is used to support the creation and management of organization trails in CloudTrail\. For more information, see [Creating a trail for an organization](creating-trail-organization.md)\.

The role permissions policy allows CloudTrail to complete the following actions on the specified resources:
+ Actions on all CloudTrail resources:
  + `All`
+ Actions on all Organizations resources:
  + `organizations:DescribeAccount`
  + `organizations:DescribeOrganizations`
  + `organizations:ListAccounts`
  + `organizations:ListAWSServiceAccessForOrganization`
+ Actions on all Organizations resources for the CloudTrail service principal: 
  + `organizations:ListDelegatedAdministrators` 

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for CloudTrail<a name="create-slr"></a>

You don't need to manually create a service\-linked role\. When you create an organization trail in the AWS Management Console, the AWS CLI, or the AWS API, CloudTrail creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create an organization trail, CloudTrail creates the service\-linked role for you again\. 

## Editing a service\-linked role for CloudTrail<a name="edit-slr"></a>

CloudTrail does not allow you to edit the AWSServiceRoleForCloudTrail service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for CloudTrail<a name="delete-slr"></a>

You don't need to manually delete the AWSServiceRoleForCloudTrail role\. If an AWS account is removed from an Organizations organization, the AWSServiceRoleForCloudTrail role is automatically removed from that AWS account\. You cannot detach or remove policies from the AWSServiceRoleForCloudTrail service\-linked role in an organization management account without removing the account from the organization\.

You can also use the IAM console, the AWS CLI or the AWS API to manually delete the service\-linked role\. To do this, you must first manually clean up the resources for your service\-linked role, and then you can manually delete it\. 

**Note**  
If the CloudTrail service is using the role when you try to delete the resources, then deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

To remove a resource being used by the AWSServiceRoleForCloudTrail role, you can do one of the following:
+ Remove the AWS account from the organization in Organizations\.
+ Update the trail so that it is no longer an organization trail\.
+ Delete the trail\.

For more information, see [Creating a trail for an organization](creating-trail-organization.md), [Updating a trail](cloudtrail-update-a-trail-console.md), and [Deleting a trail](cloudtrail-delete-trails-console.md)\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForCloudTrail service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported regions for CloudTrail service\-linked roles<a name="slr-regions"></a>

CloudTrail supports using service\-linked roles in all of the regions where CloudTrail and Organizations are both available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.