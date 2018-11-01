# Creating CloudWatch Alarms for CloudTrail Events: Examples<a name="cloudwatch-alarms-for-cloudtrail"></a>

This topic describes how to configure alarms for CloudTrail events using example scenarios\.

**Prerequisites**  
Before you can use the examples in this topic, you must:
+ Create a trail with the console or CLI\.
+ Create a log group\.
+ Specify or create an IAM role that grants CloudTrail the permissions to create a CloudWatch Logs log stream in the log group that you specify and to deliver CloudTrail events to that log stream\. The default `CloudTrail_CloudWatchLogs_Role` does this for you\.

For more information, see [Sending Events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\.

**Create a metric filter and create an alarm**  
To create an alarm, you must first create a metric filter and then configure an alarm based on the filter\. The procedures are shown for all examples\. For more information about syntax for metric filters and patterns for CloudTrail log events, see the JSON\-related sections of [Filter and Pattern Syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html) in the *Amazon CloudWatch Logs User Guide*\.

**Note**  
Instead of manually creating the following metric filters and alarms examples, you can use an AWS CloudFormation template to create them all at once\. For more information, see [Creating CloudWatch Alarms with an AWS CloudFormation Template](use-cloudformation-template-to-create-cloudwatch-alarms.md)\. 

**Topics**
+ [Example: Amazon S3 Bucket Activity](#cloudwatch-alarms-for-cloudtrail-s3-bucket-activity)
+ [Example: Security Group Configuration Changes](#cloudwatch-alarms-for-cloudtrail-security-group)
+ [Example: Network Access Control List \(ACL\) Changes](#cloudwatch-alarms-for-cloudtrail-network-acl)
+ [Example: Network Gateway Changes](#cloudwatch-alarms-for-cloudtrail-gateway-changes)
+ [Example: Amazon Virtual Private Cloud \(VPC\) Changes](#cloudwatch-alarms-for-cloudtrail-vpc-changes)
+ [Example: Amazon EC2 Instance Changes](#cloudwatch-alarms-for-cloudtrail-ec2-instance-changes)
+ [Example: EC2 Large Instance Changes](#cloudwatch-alarms-for-cloudtrail-ec2-large-instance-changes)
+ [Example: CloudTrail Changes](#cloudwatch-alarms-for-cloudtrail-cloudtrail-changes)
+ [Example: Console Sign\-In Failures](#cloudwatch-alarms-for-cloudtrail-signin)
+ [Example: Authorization Failures](#cloudwatch-alarms-for-cloudtrail-authorization-failures)
+ [Example: IAM Policy Changes](#cloudwatch-alarms-for-cloudtrail-iam-policy-changes)

## Example: Amazon S3 Bucket Activity<a name="cloudwatch-alarms-for-cloudtrail-s3-bucket-activity"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an Amazon S3 API call is made to `PUT` or `DELETE` bucket policy, bucket lifecycle, bucket replication, or to `PUT` a bucket ACL\. 

The alarm also is triggered for the CORS \(cross\-origin resource sharing\) `PUT` bucket and `DELETE` bucket events\. For more information, see [Cross\-Origin Resource Sharing](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the *Amazon Simple Storage Service Developer Guide*\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-s3-bucket-activity-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventSource = s3.amazonaws.com) && (($.eventName = PutBucketAcl) || ($.eventName = PutBucketPolicy) || ($.eventName = PutBucketCors) || ($.eventName = PutBucketLifecycle) || ($.eventName = PutBucketReplication) || ($.eventName = DeleteBucketPolicy) || ($.eventName = DeleteBucketCors) || ($.eventName = DeleteBucketLifecycle) || ($.eventName = DeleteBucketReplication)) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **S3BucketActivity**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **S3BucketActivityEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-s3-bucket-activity-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the **S3BucketActivity** filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_s3bucket.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

### Testing the Alarm for S3 Bucket Activity<a name="testing-the-created-alarm"></a>

You can test the alarm by changing the S3 bucket policy\.

**To test the alarm**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose an S3 bucket in a region that your trail is logging\. For example, if your trail is logging in the US East \(Ohio\) Region only, choose a bucket in the same region\. If your trail applies to all regions, choose an S3 bucket in any region\.

1. Choose **Permissions** and then choose **Bucket Policy**\.

1. Use the **Bucket policy editor** to change the policy and then choose **Save**\.

1. Your trail logs the `PutBucketPolicy` operation, and delivers the event to your CloudWatch Logs logs group\. The event triggers your metric alarm and CloudWatch Logs sends you a notification about the change\.

## Example: Security Group Configuration Changes<a name="cloudwatch-alarms-for-cloudtrail-security-group"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when configuration changes happen that involve security groups\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-security-group-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = AuthorizeSecurityGroupIngress) || ($.eventName = AuthorizeSecurityGroupEgress) || ($.eventName = RevokeSecurityGroupIngress) || ($.eventName = RevokeSecurityGroupEgress) || ($.eventName = CreateSecurityGroup) || ($.eventName = DeleteSecurityGroup) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **SecurityGroupEvents**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **SecurityGroupEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-security-group-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_secgroup.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: Network Access Control List \(ACL\) Changes<a name="cloudwatch-alarms-for-cloudtrail-network-acl"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when any configuration changes happen involving network ACLs\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-network-acl-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = CreateNetworkAcl) || ($.eventName = CreateNetworkAclEntry) || ($.eventName = DeleteNetworkAcl) || ($.eventName = DeleteNetworkAclEntry) || ($.eventName = ReplaceNetworkAclEntry) || ($.eventName = ReplaceNetworkAclAssociation) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **NetworkACLEvents**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **NetworkACLEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-network-acl-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_networkacl.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: Network Gateway Changes<a name="cloudwatch-alarms-for-cloudtrail-gateway-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to create, update, or delete a customer or Internet gateway\. 

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-gateway-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = CreateCustomerGateway) || ($.eventName = DeleteCustomerGateway) || ($.eventName = AttachInternetGateway) || ($.eventName = CreateInternetGateway) || ($.eventName = DeleteInternetGateway) || ($.eventName = DetachInternetGateway) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **GatewayChanges**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **GatewayEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Example: Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-gateway-changes-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_gateway.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: Amazon Virtual Private Cloud \(VPC\) Changes<a name="cloudwatch-alarms-for-cloudtrail-vpc-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to create, update, or delete an Amazon VPC, an Amazon VPC peering connection, or an Amazon VPC connection to classic Amazon EC2 instances\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-vpc-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = CreateVpc) || ($.eventName = DeleteVpc) || ($.eventName = ModifyVpcAttribute) || ($.eventName = AcceptVpcPeeringConnection) || ($.eventName = CreateVpcPeeringConnection) || ($.eventName = DeleteVpcPeeringConnection) || ($.eventName = RejectVpcPeeringConnection) || ($.eventName = AttachClassicLinkVpc) || ($.eventName = DetachClassicLinkVpc) || ($.eventName = DisableVpcClassicLink) || ($.eventName = EnableVpcClassicLink) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **VpcChanges**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **VpcEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-vpc-changes-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_vpc.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: Amazon EC2 Instance Changes<a name="cloudwatch-alarms-for-cloudtrail-ec2-instance-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to create, terminate, start, stop, or reboot an Amazon EC2 instance\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-ec2-instance-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = RunInstances) || ($.eventName = RebootInstances) || ($.eventName = StartInstances) || ($.eventName = StopInstances) || ($.eventName = TerminateInstances) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **EC2InstanceChanges**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **EC2InstanceEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-ec2-instance-changes-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_ec2instance.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: EC2 Large Instance Changes<a name="cloudwatch-alarms-for-cloudtrail-ec2-large-instance-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to create a 4x or 8x\-large Amazon EC2 instance\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-ec2-large-instance-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = RunInstances) && (($.requestParameters.instanceType = *.8xlarge) || ($.requestParameters.instanceType = *.4xlarge)) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **EC2LargeInstanceChanges**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **EC2LargeInstanceEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-ec2-large-instance-changes-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_ec2large.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: CloudTrail Changes<a name="cloudwatch-alarms-for-cloudtrail-cloudtrail-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to create, update, or delete a CloudTrail trail, or to start or stop logging a trail\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-cloudtrail-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = CreateTrail) || ($.eventName = UpdateTrail) || ($.eventName = DeleteTrail) || ($.eventName = StartLogging) || ($.eventName = StopLogging) }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **CloudTrailChanges**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **CloudTrailEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-cloudtrail-changes-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_cloudtrail.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: Console Sign\-In Failures<a name="cloudwatch-alarms-for-cloudtrail-signin"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when there are three or more sign\-in failures during a five minute period\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-signin-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.eventName = ConsoleLogin) && ($.errorMessage = "Failed authentication") }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **ConsoleSignInFailures**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **ConsoleSigninFailureCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-signin-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_consolesignin.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: Authorization Failures<a name="cloudwatch-alarms-for-cloudtrail-authorization-failures"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an unauthorized API call is made\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-authorization-failures-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   { ($.errorCode = "*UnauthorizedOperation") || ($.errorCode = "AccessDenied*") }
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **AuthorizationFailures**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **AuthorizationFailureCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-authorization-failures-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_ authorizationfailures.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.

## Example: IAM Policy Changes<a name="cloudwatch-alarms-for-cloudtrail-iam-policy-changes"></a>

Follow this procedure to create an Amazon CloudWatch alarm that is triggered when an API call is made to change an IAM policy\.

### Create a Metric Filter<a name="cloudwatch-alarms-for-cloudtrail-iam-policy-changes-metric-filter"></a>

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Logs**\.

1. In the list of log groups, select the check box next to the log group that you created for CloudTrail log events\.

1. Choose **Create Metric Filter**\.

1. On the **Define Logs Metric Filter** screen, choose **Filter Pattern** and then type the following:

   ```
   {($.eventName=DeleteGroupPolicy)||($.eventName=DeleteRolePolicy)||($.eventName=DeleteUserPolicy)||($.eventName=PutGroupPolicy)||($.eventName=PutRolePolicy)||($.eventName=PutUserPolicy)||($.eventName=CreatePolicy)||($.eventName=DeletePolicy)||($.eventName=CreatePolicyVersion)||($.eventName=DeletePolicyVersion)||($.eventName=AttachRolePolicy)||($.eventName=DetachRolePolicy)||($.eventName=AttachUserPolicy)||($.eventName=DetachUserPolicy)||($.eventName=AttachGroupPolicy)||($.eventName=DetachGroupPolicy)}
   ```

1. Choose **Assign Metric**\.

1. For **Filter Name**, type **IAMPolicyChanges**\.

1. For **Metric Namespace**, type **CloudTrailMetrics**\.

1. For **Metric Name**, type **IAMPolicyEventCount**\.

1. Choose **Show advanced metric settings**\.

1. For **Metric Value**, type **1**\.

1. Choose **Create Filter**\.

### Create an Alarm<a name="cloudwatch-alarms-for-cloudtrail-iam-policy-changes-create-alarm"></a>

After you create the metric filter, follow this procedure to create an alarm\.

1. On the **Filters for *Log\_Group\_Name*** page, next to the filter name, choose **Create Alarm**\.

1. On the **Create Alarm** page, provide the following values\.  
![\[CloudWatch Logs Create Alarm Wizard\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_alarm_wizard_ iampolicychanges.png)  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudwatch-alarms-for-cloudtrail.html)

1. Choose **Create Alarm**\.