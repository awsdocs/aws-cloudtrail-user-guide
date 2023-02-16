# Creating an access policy to grant access to accounts you own<a name="cloudtrail-sharing-logs-your-accounts"></a>

In Scenario 1, as an administrative user in Account A, you have full control over the Amazon S3 bucket to which CloudTrail writes log files for accounts B and C\. You want to share each business unit's log files back to business unit that created them\. But, you don't want a unit to be able to read any other unit's log files\. 

For example, to share Account B's log files with Account B but not with Account C, you must create a new IAM role in Account A that specifies that Account B is a trusted account\. This role trust policy specifies that Account B is trusted to assume the role created by Account A, and should look like the following example\. The trust policy is automatically created if you create the role by using the console\. If you use the SDK to create the role, you must supply the trust policy as a parameter to the `CreateRole` API\. If you use the CLI to create the role, you must specify the trust policy in the `create-role` CLI command\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::account-B-id:root"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

You must also create an access policy to specify that Account B can read from only the location to which B wrote its log files\. The access policy will look something like the following\. Note that the **Resource** ARN includes the twelve\-digit account ID for Account B, and the prefix you specified, if any, when you turned on CloudTrail for Account B during the aggregation process\. For more information about specifying a prefix, see [Turning on CloudTrail in additional accounts](turn-on-cloudtrail-in-additional-accounts.md)\. 

**Important**  
You must ensure that the prefix in the access policy is exactly the same as the prefix that you specified when you turned on CloudTrail for Account B\. If it is not, then you must edit the IAM role access policy in Account A to incorporate the actual prefix for Account B\. If the prefix in the role access policy is not exactly the same as the prefix you specified when you turned on CloudTrail in Account B, then Account B will not be able to access its log files\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "arn:aws:s3:::bucket-name/prefix/AWSLogs/account-B-id/*"
    }, 
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "arn:aws:s3:::bucket-name"
    }
  ]
}
```

The role you create for Account C will be nearly identical to the one you created for Account B\. The access policy for each role must include the appropriate account ID and prefix so that each account can read from only the location to which CloudTrail wrote that account's log files\. 

After you create roles for each account and specify the appropriate trust and access policies, and after an IAM user in each account has been granted access by the administrator of that account, an IAM user in accounts B or C can programmatically assume the role\. 

After you create roles for each account and specify the appropriate trust and access policies, an IAM user in one of the newly trusted accounts \(B or C\) must programmatically assume the role in order to read log files from the Amazon S3 bucket\. 

For more information, see [Assuming a role](cloudtrail-sharing-logs-assume-role.md)\. 