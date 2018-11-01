# Creating CloudWatch Alarms for CloudTrail Events: Additional Examples<a name="cloudwatch-alarms-for-cloudtrail-additional-examples"></a>

 [AWS Identity and Access Management \(IAM\) best practices ](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html) recommend that you do not use your root account credentials to access AWS\. Instead, you should [ create individual IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html#create-iam-users) so that you can give each user a unique set of security credentials\. The IAM Best Practices also recommend that you [ enable multi\-factor authentication \(MFA\) ](https://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html#enable-mfa-for-privileged-users) for IAM users who are allowed access to sensitive resources or APIs\. 

You can monitor whether activity in your AWS account adheres to these best practices by creating the CloudWatch alarms that notify you when root account credentials have been used to access AWS, or when API activity or console sign\-ins without MFA have occurred\. These alarms are described in this document\.

Configuring an alarm involves two main steps: 
+ Create a metric filter 
+ Create an alarm based on the filter

**Topics**
+ [Example: Monitor for Root Usage](#cloudwatch-alarms-for-cloudtrail-root-example)
+ [Example: Monitor for API Activity Without Multi\-factor Authentication \(MFA\)](#cloudwatch-alarms-for-cloudtrail-no-mfa-example)
+ [Example: Monitor for Console Sign In Without Multi\-factor Authentication \(MFA\)](#cloudwatch-alarms-for-cloudtrail-console-no-mfa-example)

## Example: Monitor for Root Usage<a name="cloudwatch-alarms-for-cloudtrail-root-example"></a>

This scenario walks you through how to use the AWS Management Console to create an Amazon CloudWatch alarm that is triggered when root \(account\) credentials are used\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-root-example-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { $.userIdentity.type = "Root" && $.userIdentity.invokedBy NOT EXISTS && $.eventType != "AwsServiceEvent" }
   ```
**Note**  
For more information about syntax for metric filters and patterns for CloudTrail log events, see the JSON\-related sections of [Filter and Pattern Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/FilterAndPatternSyntax.html) in the Amazon CloudWatch User Guide\.

1. Choose **Assign Metric**, and then on the **Create Metric Filter and Assign a Metric** screen, in the **Filter Name** box, enter **RootAccountUsage**

1. Under **Metric Details**, in the **Metric Namespace** box, enter **CloudTrailMetrics**\.

1. In the **Metric Name** field, enter **RootAccountUsageCount**\.

1. Choose **Metric Value**, and then type **1**\. 
**Note**  
If **Metric Value** does not appear, choose **Show advanced metric settings** first\.

1. When you are finished, choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-root-example-create-alarm"></a>

These steps are a continuation of the previous steps for creating a metric filter\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw-alarm-wizard-root-account-usage.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html)

1. When you are finished, choose **Create Alarm**\.

## Example: Monitor for API Activity Without Multi\-factor Authentication \(MFA\)<a name="cloudwatch-alarms-for-cloudtrail-no-mfa-example"></a>

This scenario walks you through how to use the AWS Management Console to create an Amazon CloudWatch alarm that is triggered when API calls are made without the use of multi\-factor authentication \(MFA\)\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-no-mfa-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { $.userIdentity.sessionContext.attributes.mfaAuthenticated != "true" }
   ```
**Note**  
For more information about syntax for metric filters and patterns for CloudTrail log events, see the JSON\-related sections of [Filter and Pattern Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/FilterAndPatternSyntax.html) in the Amazon CloudWatch User Guide\.

1. Choose **Assign Metric**, and then on the **Create Metric Filter and Assign a Metric** screen, in the **Filter Name** box, enter **ApiActivityWithoutMFA**\.

1. Under **Metric Details**, in the **Metric Namespace** box, enter **CloudTrailMetrics**\.

1. In the **Metric Name** box, enter **ApiActivityWithoutMFACount**\.

1. Choose **Metric Value**, and then type **1**\.
**Note**  
If **Metric Value** does not appear, choose **Show advanced metric settings** first\.

1. When you are finished, choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-no-mfa-create-alarm"></a>

These steps are a continuation of the previous steps for creating a metric filter\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw-alarm-wizard-api-without-mfa.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html)

1. When you are finished, choose **Create Alarm**\.

## Example: Monitor for Console Sign In Without Multi\-factor Authentication \(MFA\)<a name="cloudwatch-alarms-for-cloudtrail-console-no-mfa-example"></a>

This scenario walks you through how to use the AWS Management Console to create an Amazon CloudWatch alarm that is triggered when a console sign in is made without multi\-factor authentication\. 

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-console-no-mfa-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { $.eventName = "ConsoleLogin" && $.additionalEventData.MFAUsed = "No" }
   ```
**Note**  
For more information about syntax for metric filters and patterns for CloudTrail log events, see the JSON\-related sections of [Filter and Pattern Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/FilterAndPatternSyntax.html) in the Amazon CloudWatch User Guide\.

1. Choose **Assign Metric**, and then on the **Create Metric Filter and Assign a Metric** screen, in the **Filter Name** box, enter **ConsoleSignInWithoutMfa**

1. Under **Metric Details**, in the **Metric Namespace** box, enter **CloudTrailMetrics**\.

1. In the **Metric Name** field, enter **ConsoleSignInWithoutMfaCount**\.

1. Choose **Metric Value**, and then type **1**\.
**Note**  
If **Metric Value** does not appear, choose **Show advanced metric settings** first\.

1. When you are finished, choose **Create Filter**\.

### Example: Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-console-no-mfa-create-alarm"></a>

These steps are a continuation of the previous steps for creating a metric filter\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw-alarm-wizard-console-signin-without-mfa.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail-additional-examples.html)

1. When you are finished, choose **Create Alarm**\.