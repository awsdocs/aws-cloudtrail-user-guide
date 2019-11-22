# Managing Trails With the AWS CLI<a name="cloudtrail-additional-cli-commands"></a>

 The AWS CLI includes several other commands that help you manage your trails\. These commands add tags to trails, get trail status, start and stop logging for trails, and delete a trail\. You must run these commands from the same AWS Region where the trail was created \(its Home Region\)\. When using the AWS CLI, remember that your commands run in the AWS Region configured for your profile\. If you want to run the commands in a different Region, either change the default Region for your profile, or use the \-\-region parameter with the command\.

**Topics**
+ [Add one or more tags to a trail](#cloudtrail-additional-cli-commands-add-tag)
+ [List tags for one or more trails](#cloudtrail-additional-cli-commands-list-tags)
+ [Remove one or more tags from a trail](#cloudtrail-additional-cli-commands-remove-tag)
+ [Retrieving trail settings and the status of a trail](#cloudtrail-additional-cli-commands-retrieve)
+ [Configuring Insights event selectors](#configuring-insights-selector)
+ [Configuring event selectors](#configuring-event-selector-examples)
+ [Stopping and starting logging for a trail](#cloudtrail-start-stop-logging-cli-commands)
+ [Deleting a trail](#cloudtrail-delete-trail-cli)

## Add one or more tags to a trail<a name="cloudtrail-additional-cli-commands-add-tag"></a>

To add one or more tags to an existing trail, use the add\-tags command\.

The following example adds a tag with the name *Owner* and the value of *Mary* to a trail with the ARN of *arn:aws:cloudtrail:*us\-east\-2*:*123456789012*:trail/*my\-trail** in the US East \(Ohio\) Region\. 

```
aws cloudtrail add-tags --resource-id arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail --tags-list Key=Owner,Value=Mary --region us-east-2
```

If successful, this command returns nothing\.

## List tags for one or more trails<a name="cloudtrail-additional-cli-commands-list-tags"></a>

To view the tags associated with one or more existing trails, use the list\-tags command\.

The following example lists the tags for *Trail1* and *Trail2*\.

```
aws cloudtrail list-tags --resource-id-list arn:aws:cloudtrail:us-east-2:123456789012:trail/Trail1 arn:aws:cloudtrail:us-east-2:123456789012:trail/Trail2
```

If successful, this command returns output similar to the following\.

```
{
 "ResourceTagList": [
     {
         "ResourceId": "arn:aws:cloudtrail:us-east-2:123456789012:trail/Trail1",
         "TagsList": [
             {
                 "Value": "Alice",
                 "Key": "Name"
             },
             {
                 "Value": "Ohio",
                 "Key": "Location"
             }
         ]
     },
     {
         "ResourceId": "arn:aws:cloudtrail:us-east-2:123456789012:trail/Trail2",
         "TagsList": [
             {
                 "Value": "Bob",
                 "Key": "Name"
             }
         ]
     }
  ]
}
```

## Remove one or more tags from a trail<a name="cloudtrail-additional-cli-commands-remove-tag"></a>

To remove one or more tags from an existing trail, use the remove\-tags command\.

The following example removes tags with the names *Location* and *Name* from a trail with the ARN of *arn:aws:cloudtrail:*us\-east\-2*:*123456789012*:trail/*Trail1** in the US East \(Ohio\) Region\. 

```
aws cloudtrail remove-tags --resource-id arn:aws:cloudtrail:us-east-2:123456789012:trail/Trail1 --tags-list Key=Name Key=Location --region us-east-2
```

If successful, this command returns nothing\.

## Retrieving trail settings and the status of a trail<a name="cloudtrail-additional-cli-commands-retrieve"></a>

Use the `describe-trails` command to retrieve information about trails in an AWS Region\. The following example returns information about trails configured in the US East \(Ohio\) Region\.

```
aws cloudtrail describe-trails --region us-east-2
```

If the command succeeds, you see output similar to the following\. 

```
{
  "trailList": [
    {
      "Name": "my-trail",
      "S3BucketName": "my-bucket",
      "S3KeyPrefix": "my-prefix",
      "IncludeGlobalServiceEvents": true,
      "IsMultiRegionTrail": true,
      "HomeRegion": "us-east-2"
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail",
      "LogFileValidationEnabled": false,
      "HasCustomEventSelectors": false, 
      "SnsTopicName": "my-topic",
      "IsOrganizationTrail": false,
    },
    {
      "Name": "my-special-trail",
      "S3BucketName": "another-bucket",
      "S3KeyPrefix": "example-prefix",
      "IncludeGlobalServiceEvents": false,
      "IsMultiRegionTrail": false,
      "HomeRegion": "us-east-2",
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-special-trail",
      "LogFileValidationEnabled": false,
      "HasCustomEventSelectors": true,
      "IsOrganizationTrail": false
    },
    {
      "Name": "my-org-trail",
      "S3BucketName": "my-bucket",
      "S3KeyPrefix": "my-prefix",
      "IncludeGlobalServiceEvents": true,
      "IsMultiRegionTrail": true,
      "HomeRegion": "us-east-1"
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-org-trail",
      "LogFileValidationEnabled": false,
      "HasCustomEventSelectors": false, 
      "SnsTopicName": "my-topic",
      "IsOrganizationTrail": true
    }
  ]
}
```

Use the `get-trail` command to retrieve settings information about a specific trail\. The following example returns settings information for a trail named *my\-trail*\.

```
aws cloudtrail get-trail - -name my-trail
```

If successful, this command returns output similar to the following\.

```
{
   "Trail": {
      "Name": "my-trail",
      "S3BucketName": "my-bucket",
      "S3KeyPrefix": "my-prefix",
      "IncludeGlobalServiceEvents": true,
      "IsMultiRegionTrail": true,
      "HomeRegion": "us-east-2"
      "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/my-trail",
      "LogFileValidationEnabled": false,
      "HasCustomEventSelectors": false, 
      "SnsTopicName": "my-topic",
      "IsOrganizationTrail": false,
   }
}
```

Run the `get-trail-status` command to retrieve the status of a trail\. You must either run this command from the AWS Region where it was created \(the Home Region\), or you must specify that Region by using the \-\-region parameter\.

**Note**  
If the trail is an organization trail and you are a member account in the organization in AWS Organizations, you must provide the full ARN of that trail, and not just the name\.

```
aws cloudtrail get-trail-status --name my-trail
```

If the command succeeds, you see output similar to the following\. 

```
{
    "LatestDeliveryTime": 1441139757.497, 
    "LatestDeliveryAttemptTime": "2015-09-01T20:35:57Z", 
    "LatestNotificationAttemptSucceeded": "2015-09-01T20:35:57Z", 
    "LatestDeliveryAttemptSucceeded": "2015-09-01T20:35:57Z", 
    "IsLogging": true, 
    "TimeLoggingStarted": "2015-09-01T00:54:02Z", 
    "StartLoggingTime": 1441068842.76, 
    "LatestDigestDeliveryTime": 1441140723.629, 
    "LatestNotificationAttemptTime": "2015-09-01T20:35:57Z", 
    "TimeLoggingStopped": ""
}
```

In addition to the fields shown in the preceding JSON code, the status contains the following fields if there are Amazon SNS or Amazon S3 errors: 
+ `LatestNotificationError`\. Contains the error emitted by Amazon SNS if a subscription to a topic fails\.
+ `LatestDeliveryError`\. Contains the error emitted by Amazon S3 if CloudTrail cannot deliver a log file to a bucket\.

## Configuring Insights event selectors<a name="configuring-insights-selector"></a>

Enable Insights events on a trail by running the put\-insight\-selectors, and specifying `ApiCallRateInsight` as the value of the `InsightType` attribute\. To view the Insights selector settings for a trail, run the `get-insight-selectors` command\. You must either run this command from the AWS Region where the trail was created \(the Home Region\), or you must specify that Region by adding the \-\-region parameter to the command\.

### Example: A trail that logs Insights events<a name="configuring-insights-selector-example"></a>

The following example uses put\-insight\-selectors to create an Insights event selector for a trail named *TrailName3*\. This enables Insights event collection for the *TrailName3* trail\.

```
aws cloudtrail put-insight-selectors --trail-name TrailName3 --insight-selectors '{"InsightType": "ApiCallRateInsight"}'
```

The example returns the Insights event selector that is configured for the trail\.

```
{
   "InsightSelectors": [ 
      { 
         "InsightType": "ApiCallRateInsight"
      }
   ],
   "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName3"
}
```

### Example: Turn off Insights event collection for a trail<a name="configuring-insights-selector-example2"></a>

The following example uses put\-insight\-selectors to remove the Insights event selector for a trail named *TrailName3*\. Clearing the JSON string of Insights selectors disables Insights event collection for the *TrailName3* trail\.

```
aws cloudtrail put-insight-selectors --trail-name TrailName3 --insight-selectors '[]'
```

The example returns the now\-empty Insights event selector that is configured for the trail\.

```
{
   "InsightSelectors": [ 
      { 
         "InsightType": ""
      }
   ],
   "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName3"
}
```

## Configuring event selectors<a name="configuring-event-selector-examples"></a>

To view the event selector settings for a trail, run the `get-event-selectors` command\. You must either run this command from the AWS Region where it was created \(the Home Region\), or you must specify that Region by using the \-\-region parameter\. 

```
aws cloudtrail get-event-selectors --trail-name TrailName
```

**Note**  
If the trail is an organization trail and you are a member account in the organization in AWS Organizations, you must provide the full ARN of that trail, and not just the name\.

The following example returns the default settings for an event selector for a trail\.

```
{
    "EventSelectors": [
        {
            "ExcludeManagementEventSources": [],
            "IncludeManagementEvents": true,
            "DataResources": [],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

To create an event selector, run the `put-event-selectors` command\. When an event occurs in your account, CloudTrail evaluates the configuration for your trails\. If the event matches any event selector for a trail, the trail processes and logs the event\. You can configure up to 5 event selectors for a trail and up to 250 data resources for a trail\. For more information, see [Logging Data Events for Trails](logging-data-events-with-cloudtrail.md)\.

**Topics**
+ [Example: A trail with specific event selectors](#configuring-event-selector-example1)
+ [Example: A trail that logs all management and data events](#configuring-event-selector-example2)
+ [Example: A trail that does not log AWS Key Management Service events](#configuring-event-selector-example-kms)

### Example: A trail with specific event selectors<a name="configuring-event-selector-example1"></a>

The following example creates an event selector for a trail named *TrailName* to include read\-only and write\-only management events, data events for two Amazon S3 bucket/prefix combinations, and data events for a single AWS Lambda function named *hello\-world\-python\-function*\. 

```
aws cloudtrail put-event-selectors --trail-name TrailName --event-selectors '[{"ReadWriteType": "All","IncludeManagementEvents": true,"DataResources": [{"Type":"AWS::S3::Object", "Values": ["arn:aws:s3:::mybucket/prefix","arn:aws:s3:::mybucket2/prefix2"]},{"Type": "AWS::Lambda::Function","Values": ["arn:aws:lambda:us-west-2:999999999999:function:hello-world-python-function"]}]}]'
```

The example returns the event selector configured for the trail\.

```
{
    "EventSelectors": [
        {
            "ExcludeManagementEventSources": [],
            "IncludeManagementEvents": true,
            "DataResources": [
                {
                    "Values": [
                        "arn:aws:s3:::mybucket/prefix",
                        "arn:aws:s3:::mybucket2/prefix2"
                    ],
                    "Type": "AWS::S3::Object"
                },
                {
                    "Values": [
                        "arn:aws:lambda:us-west-2:123456789012:function:hello-world-python-function"
                    ],
                    "Type": "AWS::Lambda::Function"
                },
            ],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

### Example: A trail that logs all management and data events<a name="configuring-event-selector-example2"></a>

The following example creates an event selector for a trail named *TrailName2* that includes all events, including read\-only and write\-only management events, and all data events for all Amazon S3 buckets and AWS Lambda functions in the AWS account\.

**Note**  
If the trail applies only to one Region, only events in that Region are logged, even though the event selector parameters specify all Amazon S3 buckets and Lambda functions\. Event selectors apply only to the Regions where the trail is created\.

```
aws cloudtrail put-event-selectors --trail-name TrailName2 --event-selectors '[{"ReadWriteType": "All","IncludeManagementEvents": true,"DataResources": [{"Type":"AWS::S3::Object", "Values": ["arn:aws:s3:::"]},{"Type": "AWS::Lambda::Function","Values": ["arn:aws:lambda"]}]}]'
```

The example returns the event selectors configured for the trail\.

```
{
    "EventSelectors": [
        {
            "ExcludeManagementEventSources": [],
            "IncludeManagementEvents": true,
            "DataResources": [
                {
                    "Values": [
                        "arn:aws:s3:::"
                    ],
                    "Type": "AWS::S3::Object"
                },
                {
                    "Values": [
                        "arn:aws:lambda"
                    ],
                    "Type": "AWS::Lambda::Function"
                },
            ],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName2"
}
```

### Example: A trail that does not log AWS Key Management Service events<a name="configuring-event-selector-example-kms"></a>

The following example creates an event selector for a trail named *TrailName* to include read\-only and write\-only management events, but exclude AWS Key Management Service \(AWS KMS\) events\. Because AWS KMS events are treated as management events, and there can be a high volume of them, they can have a substantial impact on your CloudTrail bill if you have more than one trail that captures management events\. The user in this example has elected to exclude AWS KMS events from every trail except for one\. To exclude an event source, add `ExcludeManagementEventSources` to your event selectors, and specify an event source in the string value\. In this release, you can exclude events from `kms.amazonaws.com`\.

To start logging AWS KMS events to a trail again, pass an empty string as the value of `ExcludeManagementEventSources`\.

```
aws cloudtrail put-event-selectors --trail-name TrailName --event-selectors '[{"ReadWriteType": "All","ExcludeManagementEventSources": ["kms.amazonaws.com"],"IncludeManagementEvents": true]}]'
```

The example returns the event selector configured for the trail\.

```
{
    "EventSelectors": [
        {
            "ExcludeManagementEventSources": [ "kms.amazonaws.com" ],
            "IncludeManagementEvents": true,
            "DataResources": [],
            "ReadWriteType": "All"
        }
    ],
    "TrailARN": "arn:aws:cloudtrail:us-east-2:123456789012:trail/TrailName"
}
```

To start logging AWS KMS events to a trail again, pass an empty string as the value of `ExcludeManagementEventSources`, as shown in the following command\.

```
aws cloudtrail put-event-selectors --trail-name TrailName --event-selectors '[{"ReadWriteType": "All","ExcludeManagementEventSources": [],"IncludeManagementEvents": true]}]'
```

## Stopping and starting logging for a trail<a name="cloudtrail-start-stop-logging-cli-commands"></a>

The following commands start and stop CloudTrail logging\.

```
aws cloudtrail start-logging --name awscloudtrail-example
```

```
aws cloudtrail stop-logging --name awscloudtrail-example
```

**Note**  
Before deleting a bucket, run the `stop-logging` command to stop delivering events to the bucket\. If you don’t stop logging, CloudTrail attempts to deliver log files to a bucket with the same name for a limited period of time\.  
If you stop logging or delete a trail, CloudTrail Insights is disabled on that trail\.

## Deleting a trail<a name="cloudtrail-delete-trail-cli"></a>

You can delete a trail with the following command\. You can delete a trail only in the Region it was created \(the Home Region\)\.

```
aws cloudtrail delete-trail --name awscloudtrail-example
```

When you delete a trail, you do not delete the Amazon S3 bucket or the Amazon SNS topic associated with it\. Use the AWS Management Console, AWS CLI, or service API to delete these resources separately\.