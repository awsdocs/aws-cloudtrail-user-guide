# Creating CloudWatch alarms for CloudTrail events: examples<a name="cloudwatch-alarms-for-cloudtrail"></a>

This topic describes how to configure alarms for CloudTrail events, and includes examples\.

**Note**  
Instead of manually creating the following metric filters and alarms examples, you can use an AWS CloudFormation template to create them all at once\. For more information, see [Creating CloudWatch Alarms with an AWS CloudFormation Template](use-cloudformation-template-to-create-cloudwatch-alarms.md)\.

**Topics**
+ [Prerequisites](#cloudwatch-alarms-prerequisites)
+ [Create a metric filter and create an alarm](#cloudwatch-alarms-metric-filter-alarm)
+ [Example: Security group configuration changes](#cloudwatch-alarms-for-cloudtrail-security-group)
+ [Example: Console sign\-in failures](#cloudwatch-alarms-for-cloudtrail-signin)
+ [Example: IAM policy changes](#cloudwatch-alarms-for-cloudtrail-iam-policy-changes)

## Prerequisites<a name="cloudwatch-alarms-prerequisites"></a>

Before you can use the examples in this topic, you must:
+ Create a trail with the console or CLI\.
+ Create a log group, which you can do as part of creating a trail\.
+ Specify or create an IAM role that grants CloudTrail the permissions to create a CloudWatch Logs log stream in the log group that you specify and to deliver CloudTrail events to that log stream\. The default `CloudTrail_CloudWatchLogs_Role` does this for you\.

For more information, see [Sending Events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\. Examples in this section are performed in the Amazon CloudWatch Logs console\. For more information about how to create metric filters and alarms, see [Creating metrics from log events using filters](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html) and [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

## Create a metric filter and create an alarm<a name="cloudwatch-alarms-metric-filter-alarm"></a>

To create an alarm, you must first create a metric filter, and then configure an alarm based on the filter\. The procedures are shown for all examples\. For more information about syntax for metric filters and patterns for CloudTrail log events, see the JSON\-related sections of [Filter and pattern syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) in the *Amazon CloudWatch Logs User Guide*\.

## Example: Security group configuration changes<a name="cloudwatch-alarms-for-cloudtrail-security-group"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when configuration changes occur on security groups\.

### Create a metric filter<a name="cloudwatch-alarms-for-cloudtrail-security-group-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, choose the log group that you created for CloudTrail log events\.

1. Choose **Actions**, and then choose **Create metric filter**\.

1. On the **Define pattern** page, in **Create filter pattern**, enter the following for **Filter pattern**\.

   ```
   { ($.eventName = AuthorizeSecurityGroupIngress) || ($.eventName = AuthorizeSecurityGroupEgress) || ($.eventName = RevokeSecurityGroupIngress) || ($.eventName = RevokeSecurityGroupEgress) || ($.eventName = CreateSecurityGroup) || ($.eventName = DeleteSecurityGroup) }
   ```

1. In **Test pattern**, leave defaults\. Choose **Next**\.

1. On the **Assign metric** page, for **Filter name**, enter **SecurityGroupEvents**\.

1. In **Metric details**, turn on **Create new**, and then enter **CloudTrailMetrics** for **Metric namespace**\.

1. For **Metric name**, type **SecurityGroupEventCount**\.

1. For **Metric value**, type **1**\.

1. Leave **Default value** blank\.

1. Choose **Next**\.

1. On the **Review and create** page, review your choices\. Choose **Create metric filter** to create the filter, or choose **Edit** to go back and change values\.

### Create an alarm<a name="cloudwatch-alarms-for-cloudtrail-security-group-create-alarm"></a>

After you create the metric filter, the CloudWatch Logs log group details page for your CloudTrail trail log group opens\. Follow this procedure to create an alarm\.

1. On the **Metric filters** tab, find the metric filter you created in [Create a metric filter](#cloudwatch-alarms-for-cloudtrail-security-group-metric-filter)\. Fill the check box for the metric filter\. In the **Metric filters** bar, choose **Create alarm**\.

1. On the **Create Alarm** page, in **Specify metric and conditions**, enter the following\.

   1. For **Graph**, the line is set at **1** based on other settings you make when you create your alarm\.

   1. For **Metric name**, keep the current metric name, **SecurityGroupEventCount**\.

   1. For **Statistic**, keep the default, **Sum**\.

   1. For **Period**, keep the default, **5 minutes**\.

   1. In **Conditions**, for **Threshold type**, choose **Static**\.

   1. For **Whenever *metric\_name* is**, choose **Greater/Equal**\.

   1. For **Threshold**, enter **1**\.

   1. In **Additional configuration**, leave defaults\. Choose **Next**\.

1. On the **Configure actions** page, choose **In alarm**, which indicates that the action is taken when the threshold of 1 change event in 5 minutes is crossed, and **SecurityGroupEventCount** is in an alarm state\.

   1. For **Select an SNS topic**, choose **Create new**\.

   1. Enter **SecurityGroupChanges\_CloudWatch\_Alarms\_Topic** as the name for the new Amazon SNS topic\.

   1. In **Email endpoints that will receive the notification**, enter email addresses of users whom you want to receive notifications if this alarm is raised\. Separate email addresses with commas\.

      Email recipients specified here receive email asking them to confirm that they want to be subscribed to the Amazon SNS topic\.

   1. Choose **Create topic**\.

1. For this example, skip the other action types\. Choose **Next**\.

1. On the **Add name and description** page, enter a friendly name for the alarm, and a description\. For this example, enter **Security group configuration changes** for the name, and **Raises alarms if security group configuration changes occur** for the description\. Choose **Next**\.

1. On the **Preview and create** page, review your choices\. Choose **Edit** to make changes, or choose **Create alarm** to create the alarm\.

   After you create the alarm, CloudWatch opens the **Alarms** page\. The alarm's **Actions** column shows **Pending confirmation** until all email recipients on the SNS topic have confirmed that they want to subscribe to SNS notifications\.

## Example: Console sign\-in failures<a name="cloudwatch-alarms-for-cloudtrail-signin"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when there are three or more AWS Management Console sign\-in failures during a five minute period\.

### Create a metric filter<a name="cloudwatch-alarms-for-cloudtrail-signin-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, choose the log group that you created for CloudTrail log events\.

1. Choose **Actions**, and then choose **Create metric filter**\.

1. On the **Define pattern** page, in **Create filter pattern**, enter the following for **Filter pattern**\.

   ```
   { ($.eventName = ConsoleLogin) && ($.errorMessage = "Failed authentication") }
   ```

1. In **Test pattern**, leave defaults\. Choose **Next**\.

1. On the **Assign metric** page, for **Filter name**, enter **ConsoleSignInFailures**\.

1. In **Metric details**, turn on **Create new**, and then enter **CloudTrailMetrics** for **Metric namespace**\.

1. For **Metric name**, type **ConsoleSigninFailureCount**\.

1. For **Metric value**, type **1**\.

1. Leave **Default value** blank\.

1. Choose **Next**\.

1. On the **Review and create** page, review your choices\. Choose **Create metric filter** to create the filter, or choose **Edit** to go back and change values\.

### Create an alarm<a name="cloudwatch-alarms-for-cloudtrail-signin-create-alarm"></a>

After you create the metric filter, the CloudWatch Logs log group details page for your CloudTrail trail log group opens\. Follow this procedure to create an alarm\.

1. On the **Metric filters** tab, find the metric filter you created in [Create a metric filter](#cloudwatch-alarms-for-cloudtrail-signin-metric-filter)\. Fill the check box for the metric filter\. In the **Metric filters** bar, choose **Create alarm**\.

1. On the **Create Alarm** page, in **Specify metric and conditions**, enter the following\.

   1. For **Graph**, the line is set at **3** based on other settings you make when you create your alarm\.

   1. For **Metric name**, keep the current metric name, **ConsoleSigninFailureCount**\.

   1. For **Statistic**, keep the default, **Sum**\.

   1. For **Period**, keep the default, **5 minutes**\.

   1. In **Conditions**, for **Threshold type**, choose **Static**\.

   1. For **Whenever *metric\_name* is**, choose **Greater/Equal**\.

   1. For **Threshold**, enter **3**\.

   1. In **Additional configuration**, leave defaults\. Choose **Next**\.

1. On the **Configure actions** page, choose **In alarm**, which indicates that the action is taken when the threshold of 3 change events in 5 minutes is crossed, and **ConsoleSigninFailureCount** is in an alarm state\.

   1. For **Select an SNS topic**, choose **Create new**\.

   1. Enter **ConsoleSignInFailures\_CloudWatch\_Alarms\_Topic** as the name for the new Amazon SNS topic\.

   1. In **Email endpoints that will receive the notification**, enter email addresses of users whom you want to receive notifications if this alarm is raised\. Separate email addresses with commas\.

      Email recipients specified here receive email asking them to confirm that they want to be subscribed to the Amazon SNS topic\.

   1. Choose **Create topic**\.

1. For this example, skip the other action types\. Choose **Next**\.

1. On the **Add name and description** page, enter a friendly name for the alarm, and a description\. For this example, enter **Console sign\-in failures** for the name, and **Raises alarms if more than 3 console sign\-in failures occur in 5 minutes** for the description\. Choose **Next**\.

1. On the **Preview and create** page, review your choices\. Choose **Edit** to make changes, or choose **Create alarm** to create the alarm\.

   After you create the alarm, CloudWatch opens the **Alarms** page\. The alarm's **Actions** column shows **Pending confirmation** until all email recipients on the SNS topic have confirmed that they want to subscribe to SNS notifications\.

## Example: IAM policy changes<a name="cloudwatch-alarms-for-cloudtrail-iam-policy-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to change an IAM policy\.

### Create a metric filter<a name="cloudwatch-alarms-for-cloudtrail-iam-policy-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, choose the log group that you created for CloudTrail log events\.

1. Choose **Actions**, and then choose **Create metric filter**\.

1. On the **Define pattern** page, in **Create filter pattern**, enter the following for **Filter pattern**\.

   ```
   {($.eventName=DeleteGroupPolicy)||($.eventName=DeleteRolePolicy)||($.eventName=DeleteUserPolicy)||($.eventName=PutGroupPolicy)||($.eventName=PutRolePolicy)||($.eventName=PutUserPolicy)||($.eventName=CreatePolicy)||($.eventName=DeletePolicy)||($.eventName=CreatePolicyVersion)||($.eventName=DeletePolicyVersion)||($.eventName=AttachRolePolicy)||($.eventName=DetachRolePolicy)||($.eventName=AttachUserPolicy)||($.eventName=DetachUserPolicy)||($.eventName=AttachGroupPolicy)||($.eventName=DetachGroupPolicy)}
   ```

1. In **Test pattern**, leave defaults\. Choose **Next**\.

1. On the **Assign metric** page, for **Filter name**, enter **IAMPolicyChanges**\.

1. In **Metric details**, turn on **Create new**, and then enter **CloudTrailMetrics** for **Metric namespace**\.

1. For **Metric name**, type **IAMPolicyEventCount**\.

1. For **Metric value**, type **1**\.

1. Leave **Default value** blank\.

1. Choose **Next**\.

1. On the **Review and create** page, review your choices\. Choose **Create metric filter** to create the filter, or choose **Edit** to go back and change values\.

### Create an alarm<a name="cloudwatch-alarms-for-cloudtrail-iam-policy-changes-create-alarm"></a>

After you create the metric filter, the CloudWatch Logs log group details page for your CloudTrail trail log group opens\. Follow this procedure to create an alarm\.

1. On the **Metric filters** tab, find the metric filter you created in [Create a metric filter](#cloudwatch-alarms-for-cloudtrail-iam-policy-changes-metric-filter)\. Fill the check box for the metric filter\. In the **Metric filters** bar, choose **Create alarm**\.

1. On the **Create Alarm** page, in **Specify metric and conditions**, enter the following\.

   1. For **Graph**, the line is set at **1** based on other settings you make when you create your alarm\.

   1. For **Metric name**, keep the current metric name, **IAMPolicyEventCount**\.

   1. For **Statistic**, keep the default, **Sum**\.

   1. For **Period**, keep the default, **5 minutes**\.

   1. In **Conditions**, for **Threshold type**, choose **Static**\.

   1. For **Whenever *metric\_name* is**, choose **Greater/Equal**\.

   1. For **Threshold**, enter **1**\.

   1. In **Additional configuration**, leave defaults\. Choose **Next**\.

1. On the **Configure actions** page, choose **In alarm**, which indicates that the action is taken when the threshold of 1 change event in 5 minutes is crossed, and **IAMPolicyEventCount** is in an alarm state\.

   1. For **Select an SNS topic**, choose **Create new**\.

   1. Enter **IAM\_Policy\_Changes\_CloudWatch\_Alarms\_Topic** as the name for the new Amazon SNS topic\.

   1. In **Email endpoints that will receive the notification**, enter email addresses of users whom you want to receive notifications if this alarm is raised\. Separate email addresses with commas\.

      Email recipients specified here receive email asking them to confirm that they want to be subscribed to the Amazon SNS topic\.

   1. Choose **Create topic**\.

1. For this example, skip the other action types\. Choose **Next**\.

1. On the **Add name and description** page, enter a friendly name for the alarm, and a description\. For this example, enter **IAM Policy Changes** for the name, and **Raises alarms if IAM policy changes occur** for the description\. Choose **Next**\.

1. On the **Preview and create** page, review your choices\. Choose **Edit** to make changes, or choose **Create alarm** to create the alarm\.

   After you create the alarm, CloudWatch opens the **Alarms** page\. The alarm's **Actions** column shows **Pending confirmation** until all email recipients on the SNS topic have confirmed that they want to subscribe to SNS notifications\.