# AWS CloudTrail resource\-based policy examples<a name="security_iam_resource-based-policy-examples"></a>

**Note**  
Currently, CloudTrail Lake supports integrations in all commercial AWS Regions where CloudTrail Lake is available except for Middle East \(UAE\)\. For information about CloudTrail Lake supported Regions, see [CloudTrail Lake supported Regions](cloudtrail-lake-supported-regions.md)\. 

CloudTrail supports resource\-based permissions policies for CloudTrail channels used for CloudTrail Lake integrations\. For more information about creating integrations with CloudTrail Lake, see [Create an integration with an event source outside of AWS](query-event-data-store-integration.md)\. 

The information required for the policy is determined by the integration type\.
+ For a direction integration, CloudTrail requires the policy to contain the partner's AWS account IDs, and requires you to enter the unique external ID provided by the partner\. CloudTrail automatically adds the partner's AWS account IDs to the resource policy when you create an integration using the CloudTrail console\. Refer to the [partner's documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store-integration.html#cloudtrail-lake-partner-information#lake-integration-partner-documentation) to learn how to get the AWS account numbers required for the policy\.
+ For a solution integration, you must specify at least one AWS account ID as principal, and can optionally enter an external ID to prevent against confused deputy\.

The following are requirements for the resource\-based policy:
+ The resource ARN defined in the policy must match the channel ARN the policy is attached to\.
+  The policy contains only one action: `cloudtrail-data:PutAuditEvents` 
+  The policy contains at least one statement\. The policy can have a maximum of 20 statements\. 
+  Each statement contains at least one principal\. A statement can have a maximum of 50 principals\. 

The channel owner can call the `PutAuditEvents` API on the channel unless the policy denies the owner access to the resource\.

**Topics**
+ [Example: Providing channel access to principals](#security_iam_resource-based-policy-examples-principals)
+ [Example: Using an external ID to prevent against confused deputy](#security_iam_resource-based-policy-examples-externalID)

## Example: Providing channel access to principals<a name="security_iam_resource-based-policy-examples-principals"></a>

The following example grants permissions to the principals with the ARNs `arn:aws:iam::111122223333:root`, `arn:aws:iam::444455556666:root`, and `arn:aws:iam::123456789012:root` to call the [PutAuditEvents](https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html) API on the CloudTrail channel with the ARN `arn:aws:cloudtrail:us-east-1:777788889999:channel/EXAMPLE-80b5-40a7-ae65-6e099392355b`\. 

```
{
    "Version": "2012-10-17",
    "Statement":
    [
        {
            "Sid": "ChannelPolicy",
            "Effect": "Allow",
            "Principal":
            {
                "AWS":
                [
                    "arn:aws:iam::111122223333:root",
                    "arn:aws:iam::444455556666:root",
                    "arn:aws:iam::123456789012:root"
                ]
            },
            "Action": "cloudtrail-data:PutAuditEvents",
            "Resource": "arn:aws:cloudtrail:us-east-1:777788889999:channel/EXAMPLE-80b5-40a7-ae65-6e099392355b"
        }
    ]
}
```

## Example: Using an external ID to prevent against confused deputy<a name="security_iam_resource-based-policy-examples-externalID"></a>

The following example uses an external ID to address and prevent against [confused deputy](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cross-service-confused-deputy-prevention.html)\. The confused deputy problem is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. 

The integration partner creates the external ID to use in the policy\. Then, it provides the external ID to you as part of creating the integration\. The value can be any unique string, such as a passphrase or account number\. 

The example grants permissions to the principals with the ARNs `arn:aws:iam::111122223333:root`, `arn:aws:iam::444455556666:root`, and `arn:aws:iam::123456789012:root` to call the [PutAuditEvents](https://docs.aws.amazon.com/awscloudtraildata/latest/APIReference/API_PutAuditEvents.html) API on the CloudTrail channel resource if the call to the `PutAuditEvents` API includes the external ID value defined in the policy\. 

```
{
    "Version": "2012-10-17",
    "Statement":
    [
        {
            "Sid": "ChannelPolicy",
            "Effect": "Allow",
            "Principal":
            {
                "AWS":
                [
                    "arn:aws:iam::111122223333:root",
                    "arn:aws:iam::444455556666:root",
                    "arn:aws:iam::123456789012:root"
                ]
            },
            "Action": "cloudtrail-data:PutAuditEvents",
            "Resource": "arn:aws:cloudtrail:us-east-1:777788889999:channel/EXAMPLE-80b5-40a7-ae65-6e099392355b",
            "Condition":
            {
                "StringEquals":
                {
                    "cloudtrail:ExternalId": "uniquePartnerExternalID"
                }
            }
        }
    ]
}
```