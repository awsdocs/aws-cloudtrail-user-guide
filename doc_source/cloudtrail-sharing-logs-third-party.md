# Creating an Access Policy to Grant Access to a Third Party<a name="cloudtrail-sharing-logs-third-party"></a>

Account A must create a separate IAM role for Account Z, the third\-party analyzer in Scenario 2\. When you create the role, AWS automatically creates the trust relationship, which specifies that Account Z will be trusted to assume the role\. The access policy for the role specifies what actions Account Z can take\. For more information about creating roles and role policies, see [Creating a Role](cloudtrail-sharing-logs-create-role.md)\.

For example, the trust relationship created by AWS specifies that Account Z is trusted to assume the role created by Account A\. The following is an example trust policy:

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "",
        "Effect": "Allow",
        "Principal": {"AWS": "arn:aws:iam::account-Z-id:root"},
        "Action": "sts:AssumeRole"
    }]
}
```

If you specified an external ID when you created the role for Account Z, your access policy contains an added `Condition` element that tests the unique ID assigned by Account Z\. The test is performed when the role is assumed\. The following example access policy has a `Condition` element\.

For more information, see [How to Use an External ID When Granting Access to Your AWS Resources to a Third Party](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) in the *IAM User Guide*\.

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Sid": "",
        "Effect": "Allow",
        "Principal": {"AWS": "arn:aws:iam::account-Z-id:root"},
        "Action": "sts:AssumeRole",
        "Condition": {"StringEquals": {"sts:ExternalId": "external-ID-issued-by-account-Z"}}
    }]
}
```

You must also create an access policy for the Account A role to specify that Account Z can read all logs from the Amazon S3 bucket\. The access policy should look something like the following example\. The wild card \(\*\) at the end of the `Resource` value indicates that Account Z can access any log file in the S3 bucket to which it has been granted access\.

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
            "Resource": "arn:aws:s3:::bucket-name/*"
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

After you have created a role for Account Z and specified the appropriate trust relationship and access policy, an IAM user in Account Z must programmatically assume the role to be able to read log files from the bucket\. For more information, see [Assuming a Role](cloudtrail-sharing-logs-assume-role.md)\. 