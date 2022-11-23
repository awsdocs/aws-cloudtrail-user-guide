# Organization delegated administrator<a name="cloudtrail-delegated-administrator"></a>

When you use CloudTrail with an AWS Organizations organization, you can assign any account within the organization to act as a CloudTrail delegated administrator to manage the organization's trails and event data stores on behalf of the organization\. A delegated administrator is a member account in an organization that can perform the same administrative tasks in CloudTrail as the management account\. Only the organization management account can assign a CloudTrail delegated administrator\.

If you choose a delegated administrator, this member account has administrative permissions on all trails and event data stores in the organization\. Adding a delegated administrator does not disrupt the management or operation of the organization's trails or event data stores\.

The first time you add a delegated administrator, CloudTrail checks whether the organization’s management account has a service\-linked role\. If the management account does not have a service\-linked role, CloudTrail creates the service\-linked role for the management account\. For more information about service\-linked roles, see [Using service\-linked roles for AWS CloudTrail](using-service-linked-roles.md)\.

Take note of the following factors that define how the delegated administrator operates in CloudTrail\.

**The management account remains the owner of any CloudTrail organization resources the delegated administrator creates\.**  
The organization's management account remains the owner of any CloudTrail organization resources the delegated administrator creates, such as trails and event data stores\. This provides continuity for the organization in the event the delegated administrator changes\.

**Removing a delegated administrator account does not delete any CloudTrail organization resources they created\.**  
Organization trails and event data stores created by the delegated administrator are not deleted when you remove the delegated administrator, because the management account always serves as the owner of the CloudTrail organization resources regardless of whether they are created by the delegated administrator or the management account\.

**An organization can have a maximum of three CloudTrail delegated administrators\.**  
You can have a maximum of three CloudTrail delegated administrators per organization\. For more information about removing a delegated administrator, see [Remove a CloudTrail delegated administrator](cloudtrail-remove-delegated-administrator.md)\.

**Topics**
+ [Required permissions to assign a delegated administrator](cloudtrail-delegated-administrator-permissions.md)
+ [Add a CloudTrail delegated administrator](cloudtrail-add-delegated-administrator.md)
+ [Remove a CloudTrail delegated administrator](cloudtrail-remove-delegated-administrator.md)