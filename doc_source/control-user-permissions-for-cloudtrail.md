# Controlling User Permissions for CloudTrail<a name="control-user-permissions-for-cloudtrail"></a>

AWS CloudTrail integrates with AWS Identity and Access Management \(IAM\), which allows you to control access to CloudTrail and to other AWS resources that CloudTrail requires, including Amazon S3 buckets and Amazon Simple Notification Service \(Amazon SNS\) topics\. You can use AWS Identity and Access Management to control which AWS users can create, configure, or delete AWS CloudTrail trails, start and stop logging, and access the buckets that contain log information\. 

If you work with CloudTrail as the root user in your account, you can perform all the tasks associated with trails, including creating trails, reading logs, and so on\. If other people in your organization need to work with CloudTrail, you can create IAM users for those people and give them individual names and passwords\. When you do that, you must also give users permissions to work with CloudTrail and with any other AWS services they need to access, such as Amazon S3\. \(By default, IAM users have no permissions and cannot perform any actions in AWS\.\) 

**Important**  
We consider it a best practice not to use root account credentials to perform everyday work in AWS\. Instead, we recommend that you create an IAM administrators group with appropriate permissions, create IAM users for the people in your organization who need to perform administrative tasks \(including for yourself\), and add those users to the administrative group\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAMBestPractices.html) in the *IAM User Guide* guide\. 


+ [Granting Permissions for CloudTrail Administration](grant-permissions-for-cloudtrail-administration.md)
+ [Granting Custom Permissions for CloudTrail Users](grant-custom-permissions-for-cloudtrail-users.md)