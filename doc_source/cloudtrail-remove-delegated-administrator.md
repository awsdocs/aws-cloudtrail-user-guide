# Remove a CloudTrail delegated administrator<a name="cloudtrail-remove-delegated-administrator"></a>

You can remove a CloudTrail delegated administrator using the CloudTrail console or the AWS CLI\.

------
#### [ CloudTrail Console ]

The following procedure shows you how to remove a CloudTrail delegated administrator using the CloudTrail console\.

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Settings** in the left navigation pane of the CloudTrail console\.

1. In the **Organization delegated administrators** section, choose the delegated administrator that you want to remove\.

1.  Choose **Remove administrator**\. 

1. Confirm you want to remove the delegated administrator and then choose **Remove administrator**\.

------
#### [ AWS CLI ]

The following command removes a CloudTrail delegated administrator\.

```
aws cloudtrail deregister-organization-delegated-admin
  --delegated-admin-account-id="delegatedAdminAccountId"
```

This command produces no output if it's successful\.

------