# Sending events to CloudWatch Logs<a name="send-cloudtrail-events-to-cloudwatch-logs"></a>

When you configure your trail to send events to CloudWatch Logs, CloudTrail sends only the events that match your trail settings\. For example, if you configure your trail to log data events only, your trail sends data events only to your CloudWatch Logs log group\. CloudTrail supports sending data, Insights, and management events to CloudWatch Logs\. For more information, see [Working with CloudTrail log files](cloudtrail-working-with-log-files.md)\.

To send events to a CloudWatch Logs log group:
+ Make sure you have sufficient permissions to create or specify an IAM role\. For more information, see [Granting permission to view and configure Amazon CloudWatch Logs information on the CloudTrail console](security_iam_id-based-policy-examples.md#grant-cloudwatch-permissions-for-cloudtrail-users)\.
+ Create a new trail or specify an existing one\. For more information, see [Creating and updating a trail with the console](cloudtrail-create-and-update-a-trail-by-using-the-console.md)\.
+ Create a log group or specify an existing one\.
+ Specify an IAM role\. If you are modifying an existing IAM role for an organization trail, you must manually update the policy to allow logging for the organization trail\. For more information, see [this policy example](#policy-cwl-org) and [Creating a trail for an organization](creating-trail-organization.md)\.
+ Attach a role policy or use the default\.

**Contents**
+ [Configuring CloudWatch Logs monitoring with the console](#send-cloudtrail-events-to-cloudwatch-logs-console)
  + [Creating a log group or specifying an existing log group](#send-cloudtrail-events-to-cloudwatch-logs-console-create-log-group)
  + [Specifying an IAM role](#send-cloudtrail-events-to-cloudwatch-logs-console-create-role)
  + [Viewing events in the CloudWatch console](#viewing-events-in-cloudwatch)
+ [Configuring CloudWatch Logs monitoring with the AWS CLI](#send-cloudtrail-events-to-cloudwatch-logs-cli)
  + [Creating a log group](#send-cloudtrail-events-to-cloudwatch-logs-cli-create-log-group)
  + [Creating a role](#send-cloudtrail-events-to-cloudwatch-logs-cli-create-role)
  + [Creating a policy document](#send-cloudtrail-events-to-cloudwatch-logs-cli-create-policy-document)
  + [Updating the trail](#send-cloudtrail-events-to-cloudwatch-logs-cli-update-trail)
+ [Limitation](#send-cloudtrail-events-to-cloudwatch-logs-limitations)

## Configuring CloudWatch Logs monitoring with the console<a name="send-cloudtrail-events-to-cloudwatch-logs-console"></a>

You can use the AWS Management Console to configure your trail to send events to CloudWatch Logs for monitoring\.

### Creating a log group or specifying an existing log group<a name="send-cloudtrail-events-to-cloudwatch-logs-console-create-log-group"></a>

CloudTrail uses a CloudWatch Logs log group as a delivery endpoint for log events\. You can create a log group or specify an existing one\.

**To create or specify a log group**

1. Make sure you log in with an administrative user or role with sufficient permissions to configure CloudWatch Logs integration\. For more information, see [Granting permission to view and configure Amazon CloudWatch Logs information on the CloudTrail console](security_iam_id-based-policy-examples.md#grant-cloudwatch-permissions-for-cloudtrail-users)\.

1. Open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose the trail name\. If you choose a trail that applies to all regions, you will be redirected to the region in which the trail was created\. You can create a log group or choose an existing log group in the same region as the trail\.
**Note**  
A trail that applies to all regions sends log files from all regions to the CloudWatch Logs log group that you specify\.

1. For **CloudWatch Logs**, choose **Configure**\.

1. For **New or existing log group**, type the log group name , and then choose **Continue**\. For more information, see [CloudWatch log group and log stream naming for CloudTrail](cloudwatch-log-group-log-stream-naming-for-cloudtrail.md)\.

1. For the IAM role, choose an existing role or create one\. If you create an IAM role, type a role name\.

1. Choose **Allow** to grant CloudTrail permissions to create a CloudWatch Logs log stream and deliver events\. 

### Specifying an IAM role<a name="send-cloudtrail-events-to-cloudwatch-logs-console-create-role"></a>

You can specify a role for CloudTrail to assume to deliver events to the log stream\.

**To specify a role**

1. By default, the `CloudTrail_CloudWatchLogs_Role` is specified for you\. The default role policy has the required permissions to create a CloudWatch Logs log stream in a log group that you specify, and to deliver CloudTrail events to that log stream\. 
**Note**  
If you want to use this role for a log group for an organization trail, you must manually modify the policy after you create the role\. For more information, see [this policy example](#policy-cwl-org) and [Creating a trail for an organization](creating-trail-organization.md)\.

   1. To verify the role, go to the AWS Identity and Access Management console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. Choose **Roles** and then choose the **CloudTrail\_CloudWatchLogs\_Role**\.

   1. To see the contents of the role policy, choose **View Policy Document**\.

1. You can specify another role, but you must attach the required role policy to the existing role if you want to use it to send events to CloudWatch Logs\. For more information, see [Role policy document for CloudTrail to use CloudWatch Logs for monitoring](cloudtrail-required-policy-for-cloudwatch-logs.md)\.



### Viewing events in the CloudWatch console<a name="viewing-events-in-cloudwatch"></a>

After you configure your trail to send events to your CloudWatch Logs log group, you can view the events in the CloudWatch console\. CloudTrail typically delivers events to your log group within an average of about 15 minutes of an API call\. This time is not guaranteed\. Review the [AWS CloudTrail Service Level Agreement](http://aws.amazon.com/cloudtrail/sla) for more information\.

**To view events in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Logs**\.

1. Choose the log group that you specified for your trail\.

1. Choose the log stream name\.

1. To see the details of the event that your trail logged, choose an event\.

**Note**  
The **Time \(UTC\) **column in the CloudWatch console shows when the event was delivered to your log group\. To see the actual time that the event was logged by CloudTrail, see the `eventTime` field\.

## Configuring CloudWatch Logs monitoring with the AWS CLI<a name="send-cloudtrail-events-to-cloudwatch-logs-cli"></a>

You can use the AWS CLI to configure CloudTrail to send events to CloudWatch Logs for monitoring\.

### Creating a log group<a name="send-cloudtrail-events-to-cloudwatch-logs-cli-create-log-group"></a>

1. If you don't have an existing log group, create a CloudWatch Logs log group as a delivery endpoint for log events using the CloudWatch Logs `create-log-group` command\.

   ```
   aws logs create-log-group --log-group-name name
   ```

   The following example creates a log group named `CloudTrail/logs`:

   ```
   aws logs create-log-group --log-group-name CloudTrail/logs
   ```

1. Retrieve the log group Amazon Resource Name \(ARN\)\.

   ```
   aws logs describe-log-groups
   ```

### Creating a role<a name="send-cloudtrail-events-to-cloudwatch-logs-cli-create-role"></a>

Create a role for CloudTrail that enables it to send events to the CloudWatch Logs log group\. The IAM `create-role` command takes two parameters: a role name and a file path to an assume role policy document in JSON format\. The policy document that you use gives `AssumeRole` permissions to CloudTrail\. The `create-role` command creates the role with the required permissions\. 

To create the JSON file that will contain the policy document, open a text editor and save the following policy contents in a file called `assume_role_policy_document.json`\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudtrail.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Run the following command to create the role with `AssumeRole` permissions for CloudTrail\. 

```
aws iam create-role --role-name role_name --assume-role-policy-document file://<path to assume_role_policy_document>.json
```

When the command completes, take a note of the role ARN in the output\.

### Creating a policy document<a name="send-cloudtrail-events-to-cloudwatch-logs-cli-create-policy-document"></a>

Create the following role policy document for CloudTrail\. This document grants CloudTrail the permissions required to create a CloudWatch Logs log stream in the log group you specify and to deliver CloudTrail events to that log stream\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {

      "Sid": "AWSCloudTrailCreateLogStream2014110",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogStream"
      ],
      "Resource": [
        "arn:aws:logs:region:accountID:log-group:log_group_name:log-stream:accountID_CloudTrail_region*"
      ]

    },
    {
      "Sid": "AWSCloudTrailPutLogEvents20141101",
      "Effect": "Allow",
      "Action": [
        "logs:PutLogEvents"
      ],
      "Resource": [
        "arn:aws:logs:region:accountID:log-group:log_group_name:log-stream:accountID_CloudTrail_region*"
      ]
    }
  ]
}
```

Save the policy document in a file called `role-policy-document.json`\.<a name="policy-cwl-org"></a>

If you're creating a policy that might be used for organization trails as well, you will need to configure it slightly differently\. For example, the following policy grants CloudTrail the permissions required to create a CloudWatch Logs log stream in the log group you specify and to deliver CloudTrail events to that log stream for both trails in the AWS account 111111111111 and for organization trails created in the 111111111111 account that are applied to the AWS Organizations organization with the ID of *o\-exampleorgid*:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AWSCloudTrailCreateLogStream20141101",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogStream"
            ],
            "Resource": [
                "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:111111111111_CloudTrail_us-east-2*",
                "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:o-exampleorgid_*",
            ]
        },
        {
            "Sid": "AWSCloudTrailPutLogEvents20141101",
            "Effect": "Allow",
            "Action": [
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:111111111111_CloudTrail_us-east-2*",             
                "arn:aws:logs:us-east-2:111111111111:log-group:CloudTrail/DefaultLogGroupTest:log-stream:o-exampleorgid_*",
            ]
        }
    ]
}
```

For more information about organization trails, see [Creating a trail for an organization](creating-trail-organization.md)\.

Run the following command to apply the policy to the role\.

```
aws iam put-role-policy --role-name role_name --policy-name cloudtrail-policy --policy-document file://<path to role-policy-document>.json
```

### Updating the trail<a name="send-cloudtrail-events-to-cloudwatch-logs-cli-update-trail"></a>

Update the trail with the log group and role information using the CloudTrail `update-trail` command\.

```
aws cloudtrail update-trail --name trail_name --cloud-watch-logs-log-group-arn log_group_arn --cloud-watch-logs-role-arn role_arn
```

For more information about the AWS CLI commands, see the [AWS CloudTrail Command Line Reference](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/index.html)\. 

## Limitation<a name="send-cloudtrail-events-to-cloudwatch-logs-limitations"></a>

CloudWatch Logs and CloudWatch Events each [allow a maximum event size of 256 KB](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch_limits_cwl.html)\. Although most service events have a maximum size of 256 KB, some services still have events that are larger\. CloudTrail does not send these events to CloudWatch Logs or CloudWatch Events\.

Starting with CloudTrail event version 1\.05, events have a maximum size of 256 KB\. This is to help prevent exploitation by malicious actors, and allow events to be consumed by other AWS services, such as CloudWatch Logs and CloudWatch Events\.