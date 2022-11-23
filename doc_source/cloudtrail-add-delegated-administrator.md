# Add a CloudTrail delegated administrator<a name="cloudtrail-add-delegated-administrator"></a>

You can add a delegated administrator to manage an organization's CloudTrail resources, such as trails and event data stores\.

You can add a CloudTrail delegated administrator for your AWS organization using the CloudTrail console or the AWS CLI\.

Before you add a delegated administrator, be sure they have an account in your organization\. For information about how to create a new AWS account for your organization, see [Creating an AWS account in your organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html)\. For information about how to invite an existing AWS account to your organization, see [Inviting an AWS account to join your organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_invites.html)\.

------
#### [ CloudTrail Console ]

The following procedure shows you how to add a CloudTrail delegated administrator using the CloudTrail console\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Settings** in the left navigation pane of the CloudTrail console\.

1. In the **Organization delegated administrators** section, choose **Register administrator**\.

1. Enter the twelve\-digit AWS account ID of the account that you want to assign as the CloudTrail delegated administrator for the organization's trails and event data stores\.

1. Choose **Register administrator**\.

   After you add the delegated administrator, you only need to use the organization's management account to change or remove the delegated administrator account\.

------
#### [ AWS CLI ]

The following example adds a CloudTrail delegated administrator\.

```
aws cloudtrail register-organization-delegated-admin
  --member-account-id="memberAccountId"
```

This command produces no output if it's successful\.

------