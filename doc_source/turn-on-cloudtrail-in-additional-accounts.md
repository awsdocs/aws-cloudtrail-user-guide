# Turning on CloudTrail in additional accounts<a name="turn-on-cloudtrail-in-additional-accounts"></a>

You can use the console or the command line interface to turn on CloudTrail in additional AWS accounts\.

## Using the console to turn on CloudTrail in additional AWS accounts<a name="turn-on-cloudtrail-in-additional-accounts-console"></a>

You can use the CloudTrail console to turn on CloudTrail in additional accounts\.

1. Sign into the AWS management console using account 222222222222 credentials and open the AWS CloudTrail console\. In the navigation bar, select the region where you want to turn on CloudTrail\.

1. Choose **Get Started Now**\.

1. On the following page, type a name for your trail in the **Trail name** box\.

1. For **Create a new S3 bucket?**, choose **No**\. Use the text box to enter the name of the bucket you created previously for storing log files when you signed in using account 111111111111 credentials\. CloudTrail displays a warning asking you if you are sure that you want to specify an S3 bucket in another account\. Verify the name of the bucket you entered\.

1. Choose **Advanced**\.

1. In the **Log file prefix** field, enter the same prefix you entered for storing log files when you turned on CloudTrail using account 111111111111 credentials\. If you choose to use a prefix that is different from the one you entered when you turned on CloudTrail in the first account, you must edit the bucket policy on your destination bucket to allow CloudTrail to write log files to your bucket using this new prefix\.

1. \(Optional\) Choose **Yes** or **No** for **SNS notification for every log file delivery?**\. If you chose **Yes**, type a name for your Amazon SNS topic in the **SNS topic \(new\) field**\. 
**Note**  
Amazon SNS is a regional service, so if you choose to create a topic, that topic will exist in the same region in which you turn on CloudTrail\. If you have a trail that applies to all regions, you can pick an Amazon SNS topic in any region as long as you have the correct policy applied to the topic\. For more information, see [Amazon SNS topic policy for CloudTrail](cloudtrail-permissions-for-sns-notifications.md)\.

1. Choose **Turn On**\.

CloudTrail starts publishing log files that show the AWS calls made in your accounts in this region\. CloudTrail typically delivers logs within an average of about 15 minutes of an API call\. This time is not guaranteed\. Review the [AWS CloudTrail Service Level Agreement](http://aws.amazon.com/cloudtrail/sla) for more information\.

## Using the CLI to turn on CloudTrail in additional AWS accounts<a name="turn-on-cloudtrail-in-additional-accounts-cli"></a>

You can use the AWS command line tools to turn on CloudTrail in additional accounts and aggregate their log files to one Amazon S3 bucket\. For more information about these tools, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. 

Turn on CloudTrail in your additional accounts by using the `create-trail` command, specifying the following:
+ `--name` specifies the name of the trail\. 
+ `--s3-bucket-name` specifies the existing Amazon S3 bucket you created when you turned on CloudTrail in your first account \(111111111111 in this example\)\. 
+ `--s3-prefix` specifies a prefix for the log file delivery path \(optional\)\.
+ `--is-multi-region-trail` specifies that this trail will log events in all AWS Regions\. 

In contrast to trails that you create using the console, you must give every trail you create with the AWS CLI a name\. You can create one trail for each region in which an account is running AWS resources\. 

The following example command shows how to create a trail for your additional accounts by using the AWS CLI\. To have log files for these account delivered to the bucket you created in your first account \(111111111111 in this example\), specify the bucket name in the `--s3-bucket-name` option\. Amazon S3 bucket names are globally unique\. 

```
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail
```

When you run the command, you will see output similar to the following:

```
{
    "IncludeGlobalServiceEvents": true, 
    "Name": "AWSCloudTrailExample", 
    "TrailARN": "arn:aws:cloudtrail:us-east-2:222222222222:trail/my-trail", 
    "LogFileValidationEnabled": false, 
    "IsMultiRegionTrail": true, 
    "IsOrganizationTrail": false,
    "S3BucketName": "MyBucketBelongingToAccount111111111111"
}
```

For more information about using CloudTrail from the AWS command line tools, see the [CloudTrail command line reference](https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/index.html)\. 