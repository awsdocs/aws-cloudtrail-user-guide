# Creating CloudWatch alarms with an AWS CloudFormation template<a name="use-cloudformation-template-to-create-cloudwatch-alarms"></a>

After you configure your trail to deliver log files to your CloudWatch log group, you can create CloudWatch metric filters and alarms to monitor the events in the log files\. For example, you can specify an event such as the Amazon EC2 `RunInstances` operation, so that CloudWatch sends you notifications when that event occurs in your account\. You can create your filters and alarms separately or use the AWS CloudFormation template to define them all at once\.

You can use the example CloudFormation template as is, or as a reference to create your own template\. 

**Topics**
+ [Example CloudFormation template](#use-cloudformation-template-to-create-cloudwatch-alarms-example)
+ [Creating a CloudFormation stack with the template](#use-cloudformation-template-create-a-stack)
+ [CloudFormation template contents](#use-cloudformation-template-template-contents)

## Example CloudFormation template<a name="use-cloudformation-template-to-create-cloudwatch-alarms-example"></a>

The CloudFormation template has predefined CloudWatch metric filters and alarms, so that you receive email notifications when specific security\-related API calls are made in your AWS account\. 

The template is available in a zip file at the following location: [CloudWatch\_Alarms\_for\_CloudTrail\_API\_Activity\.zip](samples/CloudWatch_Alarms_for_CloudTrail_API_Activity.zip)\.

The template defines metric filters that monitor create, delete, and update operations for the following resource types:
+ Amazon EC2 instances
+ IAM policies
+ Internet gateways
+ Network ACLs
+ Security groups

When an API call occurs in your account, a metric filter monitors that API call\. If the API call exceeds the thresholds that you specify, this triggers the alarm and CloudWatch sends you an email notification\.

By default, most of the filters in the template trigger an alarm when a monitored event occurs within a five\-minute period\. You can modify these alarm thresholds for your own requirements\. For example, you can monitor for three events in a ten\-minute period\. To make the changes, edit the template or, after uploading the template, specify the thresholds in the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.

**Note**  
Because CloudTrail typically delivers log files every five minutes, specify alarm periods of five minutes or more\. 

To see the metric filters and alarms in the template, and the API calls that trigger email notifications, see [CloudFormation template contents](#use-cloudformation-template-template-contents)\. 

## Creating a CloudFormation stack with the template<a name="use-cloudformation-template-create-a-stack"></a>

A CloudFormation stack is a collection of related resources that you provision and update as a single unit\. The following procedure describes how to create the stack and validate the email address that receives notifications\.

**To create a CloudFormation stack with the template**

1. Configure your trail to deliver log files to your CloudWatch Logs log group\. See [Sending events to CloudWatch Logs](send-cloudtrail-events-to-cloudwatch-logs.md)\.

1. Download the CloudFormation template: [CloudWatch\_Alarms\_for\_CloudTrail\_API\_Activity\.zip](samples/CloudWatch_Alarms_for_CloudTrail_API_Activity.zip)\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create Stack**\.  
![\[Create an AWS CloudFormation stack in the console.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_createstack.png)

1. On the **Select Template** page, for **Name**, type a stack name\. For example, `CloudWatchAlarmsForCloudTrail`\.  
![\[Choose a template in the AWS CloudFormation console.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_select_template.png)

1. For **Source**, choose **Upload a template to Amazon S3**\.  
![\[Upload a template to the AWS CloudFormation console.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_upload_template.png)

1. Choose **Choose File**, and then select the AWS CloudFormation template that you downloaded\.

1. Choose **Next**\.

1. On the **Specify Parameters** page, for **Email**, type the email address to receive notifications\.

   

1. For **LogGroupName**, type the name of the log group that you specified when you configured your trail to deliver log files to CloudWatch Logs\.  
![\[Specify the email address and log group name for the AWS CloudFormation stack.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_parameters.png)

1. Choose **Next**\.

1. For **Options**, you can create tags or configure other advanced options\. These are not required\.  
![\[Specify additional options for the AWS CloudFormation stack.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_options.png)

1. Choose **Next**\.

1. On the **Review** page, verify that your settings are correct\.  
![\[Review the AWS CloudFormation stack before creating it.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_review.png)

1. Choose **Create**\. The stack is created in a few minutes\.  
![\[The AWS CloudFormation stack is created.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_stack_create_complete.png)

1. After the stack is created, you will receive an email at the address that you specified\.

1. In the email, choose **Confirm subscription**\. You receive email notifications when the alarms specified by the template are triggered\.  
![\[Confirm the subscription in your email.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_confirm_subscription.png)  
![\[See the following email notification example.\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/cw_cfntemplate_sample_notification.png)

## CloudFormation template contents<a name="use-cloudformation-template-template-contents"></a>

The following tables show the metric filters and alarms in the template, their purpose, and the API calls that trigger email notifications\. Notifications are triggered when one or more of the API calls for a listed filter occur in your account\. 

You can review the metric filter or alarm definitions in the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/)\.


**Network events**  

| Metric Filter and Alarm | Monitor and Send Notifications for: | Notifications triggered by one or more of the following API operations: | 
| --- | --- | --- | 
|  **SecurityGroupChangesMetricFilter** **SecurityGroupChangesAlarm**  |  API calls that create, update, and delete security groups\.  |  `CreateSecurityGroup` `DeleteSecurityGroup` `AuthorizeSecurityGroupEgress` `RevokeSecurityGroupEgress` `AuthorizeSecurityGroupIngress` `RevokeSecurityGroupIngress`  | 
|  **NetworkAclChangesMetricFilter** **NetworkAclChangesAlarm**  |  API calls that create, update, and delete network ACLs\.  |  `CreateNetworkAcl` `DeleteNetworkAcl` `CreateNetworkAclEntry` `DeleteNetworkAclEntry` `ReplaceNetworkAclAssociation` `ReplaceNetworkAclEntry`  | 
|  **GatewayChangesMetricFilter** **GatewayChangesAlarm**  |  API calls that create, update, and delete customer and internet gateways\.  |  `CreateCustomerGateway` `DeleteCustomerGateway` `AttachInternetGateway` `CreateInternetGateway` `DeleteInternetGateway` `DetachInternetGateway`  | 
|  **VpcChangesMetricFilter** **VpcChangesAlarm**  |  API calls that create, update, and delete virtual private clouds \(VPCs\), VPC peering connections, and VPC connections to classic EC2 instances using `ClassicLink`\.  |  `CreateVpc` `DeleteVpc` `ModifyVpcAttribute` `AcceptVpcPeeringConnection` `CreateVpcPeeringConnection` `DeleteVpcPeeringConnection` `RejectVpcPeeringConnection` `AttachClassicLinkVpc` `DetachClassicLinkVpc` `DisableVpcClassicLink` `EnableVpcClassicLink`  | 


**Amazon EC2 events**  

| Metric Filter and Alarm | Monitor and Send Notifications for: | Notifications triggered by one or more of the following API operations: | 
| --- | --- | --- | 
|  **EC2InstanceChangesMetricFilter** **EC2InstanceChangesAlarm**  |  The creation, termination, start, stop, and reboot of EC2 instances\.  |  `RebootInstances` `RunInstances` `StartInstances` `StopInstances` `TerminateInstances`  | 
|  **EC2LargeInstanceChangesMetricFilter** **EC2LargeInstanceChangesAlarm**  |  The creation, termination, start, stop, and reboot of 4x and 8x large EC2 instances\.  |  **At least one of the following API operations:** `RebootInstances` `RunInstances` `StartInstances` `StopInstances` `TerminateInstances` **and at least one of the following instance types:** instancetype=\*\.4xlarge instancetype=\*\.8xlarge  | 


**CloudTrail and IAM events**  

| Metric Filter and Alarm | Monitor and Send Notifications for: | Notifications triggered by one or more of the following API operations: | 
| --- | --- | --- | 
|  **CloudTrailChangesMetricFilter** **CloudTrailChangesAlarm**  |  Creating, deleting, and updating trails\. The occurrence of starting and stopping logging for a trail\.  |  `CreateTrail` `DeleteTrail` `StartLogging` `StopLogging` `UpdateTrail`  | 
|  **ConsoleSignInFailuresMetricFilter** **ConsoleSignInFailuresAlarm**  |  Console login failures  |  `eventName` is `ConsoleLogin`  and  `errorMessage` is "Failed authentication"  | 
|  **AuthorizationFailuresMetricFilter** **AuthorizationFailuresAlarm**  |  Authorization failures  |  Any API call that results in an error code: `AccessDenied`  or  `*UnauthorizedOperation`\.  | 
|  **IAMPolicyChangesMetricFilter** **IAMPolicyChangesAlarm**  |  Changes to IAM policies  |  `AttachGroupPolicy` `DeleteGroupPolicy` `DetachGroupPolicy` `PutGroupPolicy` `CreatePolicy` `DeletePolicy` `CreatePolicyVersion` `DeletePolicyVersion` `AttachRolePolicy` `DeleteRolePolicy` `DetachRolePolicy` `PutRolePolicy` `AttachUserPolicy` `DeleteUserPolicy` `DetachUserPolicy` `PutUserPolicy`  | 