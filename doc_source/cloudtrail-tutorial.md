# Getting Started with AWS CloudTrail Tutorial<a name="cloudtrail-tutorial"></a>

If you're new to AWS CloudTrail, this tutorial helps you learn how to use its features\. In this tutorial, you review your recent AWS account activity in the CloudTrail console and examine an event\. You then create a trail, which is an ongoing record of management event activity that is stored in an Amazon S3 bucket\. Unlike Event history, this ongoing record is not limited to 90 days, logs events in all AWS Regions, and can help you meet your security and auditing needs over time\.

**Topics**
+ [Prerequisites](#tutorial-prerequisites)
+ [Step 1: Review AWS Account Activity in Event History](#tutorial-step1)
+ [Step 2: Create Your First Trail](#tutorial-step2)
+ [Step 3: View Your Log Files](#tutorial-step3)
+ [Step 4: Plan For Next Steps](#tutorial-step4)

## Prerequisites<a name="tutorial-prerequisites"></a>

Before you begin, you must complete the following prerequisites and setup:
+ Create an AWS account, if you do not already have one\.

  If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

  1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

  1. Follow the online instructions\.

     Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.
+ Create an IAM user for administering CloudTrail\. For more information, see [Granting Permissions for CloudTrail Administration ](security_iam_id-based-policy-examples.md#grant-permissions-for-cloudtrail-administration)\.

## Step 1: Review AWS Account Activity in Event History<a name="tutorial-step1"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in any AWS service that supports CloudTrail, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. In other words, you can view, search, and download recent events in your AWS account before creating a trail, although creating a trail is vitally important for long\-term records and auditing of your AWS account activity\. Unlike a trail, **Event history** is limited to recent events\.

1. Sign in to the AWS Management Console using the IAM user you configured for CloudTrail administration\. Open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/home/](https://console.aws.amazon.com/cloudtrail/home/)\.

1. Review the information in your dashboard about the most recent events that have occurred in your AWS account\. One of these events should be a ConsoleSignin event, showing that you just signed in to the AWS Management Console\.  
![\[The CloudTrail dashboard showing recent events\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cloudtrail-dashboard.png)

1. To see more information about an event, expand it\.  
![\[The CloudTrail dashboard showing expanded information about an event\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cloudtrail-dashboard-expand-event.png)

1. In the navigation pane, choose **Event history**\. You see a filtered list of events, with the most recent events showing first\. The default filter for events is **Read only**, set to **false**\. You can clear that filter by choosing the delete icon\.  
![\[The CloudTrail Event history page highlighting the Read only filter\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cloudtrail-event-history.png)

1. Many more events are shown without the default filter\. You can filter events in many ways\. For example, to view all console login events, you could choose the **Event name** filter, and specify **ConsoleLogin**\. The choice of filters is up to you\.  
![\[The CloudTrail Event history page with the default filter removed, showing a partial list of filter options\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cloudtrail-event-history-filters.png)

1. You can save event history by downloading it as a file in CSV or JSON format\.  
![\[The CloudTrail Event history page showing the download options\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cloudtrail-event-history-download.png)

For more information, see [Viewing Events with CloudTrail Event History](view-cloudtrail-events.md)\.

## Step 2: Create Your First Trail<a name="tutorial-step2"></a>

While the events provided in the Event history view in the CloudTrail console are useful for reviewing recent activity, they are limited to recent activity, and they do not include all possible events that can be recorded by CloudTrail\. Additionally, your view of events in the console is limited to the AWS Region where you are signed in\. To create an ongoing record of activity in your AWS account that captures information for all AWS Regions, create a trail\. For your first trail, we recommend creating a trail that logs all [management events](cloudtrail-concepts.md#cloudtrail-concepts-management-events) in all AWS Regions, and does not log any [data events](cloudtrail-concepts.md#cloudtrail-concepts-data-events)\. Examples of management events include security events such as IAM `CreateUser` and `AttachRolePolicy` events, resource events such as `RunInstances` and `CreateBucket`, and many more\. You will create an Amazon S3 bucket where you will store the log files for the trail as part of creating the trail in the CloudTrail console\.

**Note**  
This tutorial assumes you are creating your first trail\. Depending on the number of trails you have in your AWS account, and how those trails are configured, the following procedure might or might not incur expenses\. In addition, CloudTrail stores log files in an Amazon S3 bucket\. For more information about pricing, see [AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/) and [Amazon S3 Pricing](https://aws.amazon.com/s3/pricing/)\.

1. Sign in to the AWS Management Console using the IAM user you configured for CloudTrail administration\. Open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/home/](https://console.aws.amazon.com/cloudtrail/home/)\. In the Region selector, choose the AWS Region where you want your trail to be created\. This is the Home Region for the trail\. 
**Note**  
The Home Region is the only AWS Region where you can view and update the trail after it is created, even if the trail logs events in all AWS Regions\.

1. In the navigation pane, choose **Trails**\. On the Trails page, choose **Get Started Now**\. If you do not see that option, choose **Create Trail**\.

1. In **Trail name**, give your trail a name, such as *My\-Management\-Events\-Trail*\. As a best practice, use a name that quickly identifies the purpose of the trail\. In this case, you're creating a trail that logs management events\.

1. In **Management Events**, make sure **Read/Write events** is set to **All**\. 

1. In **Data Events**, do not make any changes\. This trail will not log any data events\.

1. In **Storage Location**, in **Create a new S3 bucket**, choose **Yes**\. In **S3 bucket**, give your bucket a name, such as *my\-bucket\-for\-storing\-cloudtrail\-logs*\.
**Note**  
The name of your Amazon S3 bucket must be globally unique\. For more information, see [Amazon S3 Bucket Naming Requirements](cloudtrail-s3-bucket-naming-requirements.md)\.

1. Choose **Create**\.

## Step 3: View Your Log Files<a name="tutorial-step3"></a>

Within 15 minutes of creating your first trail, CloudTrail delivers the first set of log files to the Amazon S3 bucket for your trail\. You can look at these files and learn about the information they contain\.

1. In the navigation pane, choose **Trails**\. On the Trails page, find the name of the trail you just created \(in the example, *My\-Management\-Events\-Trail*\)\.
**Note**  
Make sure you are still signed in using the IAM user you configured for CloudTrail administration\. Otherwise you might not have sufficient permissions to view trails in the CloudTrail console or the Amazon S3 bucket that contains log files for that trail\.

1. In the row for that trail, find the value for the S3 bucket \(in the example, *my\-bucket\-for\-storing\-cloudtrail\-logs*\)\. Choose it\.

1. The Amazon S3 console opens and shows that bucket, at the top level for log files\. Because you created a trail that logs events in all AWS Regions, the display opens at the level that shows you each Region folder\. The hierarchy of the Amazon S3 bucket navigation at this level is *bucket\-name*/AWSLogs/*AWS\-account\-id/*CloudTrail\. Choose the folder for the AWS Region where you want to review log files\. For example, if you want to review the log files for the US East \(Ohio\) Region, choose **us\-east\-2**\.  
![\[An Amazon S3 bucket for a trail, showing the structure for log files in AWS Regions\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cloudtrail-trail-bucket-1.png)

1. Navigate the bucket folder structure to the year, the month, and the day where you want to review logs of activity in that Region\. In that day, there are a number of files\. The name of the files begin with your AWS account ID, and end with the extension \.gz\. For example, if your account ID is *123456789012*, you would see files with names similar to this: *123456789012*\_CloudTrail\_*us\-east\-2*\_*20190610T1255abcdeEXAMPLE*\.json\.gz\.

   To view these files, you can download them, unzip them, and then view them in a plain\-text editor or a JSON file viewer\. Some browsers also support viewing \.gz and JSON files directly\. We recommend using a JSON viewer, as it makes it easier to parse the information in CloudTrail log files\. 

   As you're browsing through the file content, you might start to wonder about what you're seeing\. CloudTrail logs events for every AWS service that experienced activity in that AWS Region at the time that event occurred\. In other words, events for different AWS services are mixed together, based solely on time\. To learn more about what a specific AWS service logs with CloudTrail, including examples of log file entries for API calls for that service, see the [list of supported services for CloudTrail](cloudtrail-aws-service-specific-topics.md#cloudtrail-aws-service-specific-topics-list), and read the CloudTrail integration topic for that service\. You can also learn more about the content and structure of CloudTrail log files by reviewing the [CloudTrail Log Event Reference](cloudtrail-event-reference.md)\.

   You might also notice what you're not seeing in log files in US East \(Ohio\)\. Specifically, you won't see any console sign\-in events, even though you know you logged into the console\. That's because console sign\-in and IAM events are [global service events](cloudtrail-concepts.md#cloudtrail-concepts-global-service-events), and are logged in a specific AWS Region\. In this case, they are logged in US East \(N\. Virginia\), which corresponds to the folder **us\-east\-1**\. Navigate to that folder, and to the year, month, and day you're interested in\. Browse the log files, and you find `ConsoleLogin` events that look similar to the following:

   ```
   {
       "eventVersion": "1.05",
       "userIdentity": {
           "type": "IAMUser",
           "principalId": "AKIAIOSFODNN7EXAMPLE",
           "arn": "arn:aws:iam::123456789012:user/Mary_Major",
           "accountId": "123456789012",
           "userName": "Mary_Major"
       },
       "eventTime": "2019-06-10T17:14:09Z",
       "eventSource": "signin.amazonaws.com",
       "eventName": "ConsoleLogin",
       "awsRegion": "us-east-1",
       "sourceIPAddress": "203.0.113.67",
       "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0",
       "requestParameters": null,
       "responseElements": {
           "ConsoleLogin": "Success"
       },
       "additionalEventData": {
           "LoginTo": "https://console.aws.amazon.com/console/home?state=hashArgs%23&isauthcode=true",
           "MobileVersion": "No",
           "MFAUsed": "No"
       },
       "eventID": "2681fc29-EXAMPLE",
       "eventType": "AwsConsoleSignIn",
       "recipientAccountId": "123456789012"
   }
   ```

   You'll notice that this log file entry tells you more than just the identity of the IAM user who logged in \(Mary\_Major\), the date and time she logged in, and that the login was successful\. You can also learn the IP address she logged in from, the operating system and browser software of the computer she used, and that she was not using multi\-factor authentication\. 

## Step 4: Plan For Next Steps<a name="tutorial-step4"></a>

Now that you have a trail, you have access to an ongoing record of events and activities in your AWS account\. This ongoing record helps you meet accounting and auditing needs for your AWS account\. However, there is a lot more you can do with CloudTrail and CloudTrail data\.
+ **Add additional security for your trail data\.** CloudTrail automatically applies a certain level of security when you create a trail\. However, there are additional steps you can take to help keep your data secure\.
  + By default, the Amazon S3 bucket you created as part of creating a trail has a policy applied that allows CloudTrail to write log files to that bucket\. The bucket is not publicly accessible, but it might be accessible to other users in your AWS account if they have permissions to read and write to buckets in your AWS account\. Review the policy for your bucket and if necessary, make changes to restrict access to a specific set of IAM users\. For more information, see the [Amazon S3 security documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/security.html) and the [example walkthrough for securing a bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/walkthrough1.html)\.
  + The log files delivered by CloudTrail to your bucket are encrypted by Amazon [server\-side encryption with Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html)\. To provide a security layer that is directly manageable, you can instead use [server\-side encryption with AWS KMS–managed keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingKMSEncryption.html) for your CloudTrail log files\. To use SSE\-KMS with CloudTrail, you create and manage a KMS key, also known as a [customer master key \(CMK\)](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)\. For more information, see [Encrypting CloudTrail Log Files with AWS KMS–Managed Keys \(SSE\-KMS\)](encrypting-cloudtrail-log-files-with-aws-kms.md)\.
  + For additional security planning, review the [security best practices for CloudTrail](best-practices-security.md)\.
+ **Create a trail to log data events\.** If you are interested in logging when objects are added, retrieved, and deleted in one or more Amazon S3 buckets, or when one or more AWS Lambda functions are invoked, these are data events\. The management event trail you created earlier in this tutorial doesn't log these types of events\. You can create a separate trail specifically to log data events for some or all of your Amazon S3 and Lambda resources\. For more information, see [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.
**Note**  
Additional charges apply for logging data events\. For more information, see [ AWS CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)\.
+ **Set up CloudWatch Logs alarms to alert you when certain events occur\.** CloudWatch Logs allows you to monitor and receive alerts for specific events captured by CloudTrail\. For example, you can monitor key security and network\-related management events, such as [failed AWS console sign\-in events](cloudwatch-alarms-for-cloudtrail.md#cloudwatch-alarms-for-cloudtrail-signin), [API calls that failed due to authorization failures](cloudwatch-alarms-for-cloudtrail.md#cloudwatch-alarms-for-cloudtrail-authorization-failures), or [changes to Amazon EC2 instances](cloudwatch-alarms-for-cloudtrail.md#cloudwatch-alarms-for-cloudtrail-ec2-instance-changes)\. For more information, see [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md)\.
+ **Use analysis tools to identify trends in your CloudTrail logs\.** While the filters in Event history can help you find specific events or event types in your recent activity, it does not provide the ability to search through activity over longer time periods\. For deeper and more sophisticated analysis, you can use Amazon Athena\. For more information, see [Querying AWS CloudTrail Logs](https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html) in the Amazon Athena User Guide\.