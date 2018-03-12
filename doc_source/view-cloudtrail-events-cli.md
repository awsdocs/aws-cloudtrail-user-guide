# Viewing CloudTrail Events with the AWS CLI<a name="view-cloudtrail-events-cli"></a>

You can look up CloudTrail events for the last 90 days using the aws cloudtrail lookup\-events command\. `lookup-events` has the following options:

+ `--max-results`

+ `--start-time`

+ `--lookup-attributes`

+ `--next-token`

+ `--generate-cli-skeleton`

+ `--cli-input-json`

These options are explained in this topic\. For a list of services supported for event lookup, see [Services Supported by CloudTrail Event History](view-cloudtrail-events-supported-services.md)\. For a list of regions supported for event lookup, see [Regions Supported by CloudTrail Event History](view-cloudtrail-events-supported-regions.md)\. 

For general information on using the AWS Command Line Interface, see the [AWS Command Line Interface User Guide](http://docs.aws.amazon.com/cli/latest/userguide/)\.



## Prerequisites<a name="aws-cli-prerequisites-for-aws-cloudtrail"></a>

+ To run AWS CLI commands, you must install the AWS CLI\. For information, see [Installing the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.

+ Make sure your AWS CLI version is greater than 1\.6\.6\. To verify the CLI version, run aws \-\-version on the command line\.

+ To set the account, region, and default output format for an AWS CLI session, use the aws configure command\. For more information, see [ Configuring the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

**Note**  
The CloudTrail AWS CLI commands are case\-sensitive\.

## Getting command line help<a name="getting-command-line-help"></a>

To see the command line help for `lookup-events`, type the following command:

```
aws cloudtrail lookup-events help
```

## Looking up events<a name="looking-up-events-with-the-aws-cli"></a>

To see the ten latest events, type the following command:

```
aws cloudtrail lookup-events
```

 A returned event looks similar to the following fictitious example, which has been formatted for readability:

```
{
    "NextToken": "kbOt5LlZe++mErCebpy2TgaMgmDvF1kYGFcH64JSjIbZFjsuvrSqg66b5YGssKutDYIyII4lrP4IDbeQdiObkp9YAlju3oXd12juy3CIZW8=", 
    "Events": [
        {
            "EventId": "0ebbaee4-6e67-431d-8225-ba0d81df5972", 
            "Username": "root", 
            "EventTime": 1424476529.0, 
            "CloudTrailEvent": "{
                  \"eventVersion\":\"1.02\",
                  \"userIdentity\":{
                        \"type\":\"Root\",
                        \"principalId\":\"111122223333\",
                        \"arn\":\"arn:aws:iam::111122223333:root\",
                        \"accountId\":\"111122223333\"},
                  \"eventTime\":\"2015-02-20T23:55:29Z\",
                  \"eventSource\":\"signin.amazonaws.com\",
                  \"eventName\":\"ConsoleLogin\",
                  \"awsRegion\":\"us-east-2\",
                  \"sourceIPAddress\":\"203.0.113.4\",
                  \"userAgent\":\"Mozilla/5.0\",
                  \"requestParameters\":null,
                  \"responseElements\":{\"ConsoleLogin\":\"Success\"},
                  \"additionalEventData\":{
                         \"MobileVersion\":\"No\",
                         \"LoginTo\":\"https://console.aws.amazon.com/console/home",
                         \"MFAUsed\":\"No\"},
                  \"eventID\":\"0ebbaee4-6e67-431d-8225-ba0d81df5972\",
                  \"eventType\":\"AwsApiCall\",
                  \"recipientAccountId\":\"111122223333\"}", 
            "EventName": "ConsoleLogin", 
            "Resources": []
        }
    ]
}
```

For an explanation of the lookup\-related fields in the output, see the section [Lookup Output Fields](#view-cloudtrail-events-cli-output-fields) later in this document\. For an explanation of the fields in the CloudTrail event, see [CloudTrail Record Contents](cloudtrail-event-reference-record-contents.md)\.

## Specifying the number of events to return<a name="specify-the-number-of-events-to-return"></a>

To specify the number of events to return, type the following command:

```
aws cloudtrail lookup-events --max-results <integer>
```

The default value for *<integer>* is 10\. Possible values are 1 through 50\. The following example returns one result\.

```
aws cloudtrail lookup-events --max-results 1
```

## Looking up events by time range<a name="look-up-events-by-time-range"></a>

Events from the past 90 days are available for lookup\. To specify a time range, type the following command:

```
aws cloudtrail lookup-events --start-time <timestamp> --end-time <timestamp>
```

`--start-time <timestamp>` specifies that only events that occur after or at the specified time are returned\. If the specified start time is after the specified end time, an error is returned\.

`--end-time <timestamp>` specifies that only events that occur before or at the specified time are returned\. If the specified end time is before the specified start time, an error is returned\.

The default start time is the earliest date that data is available within the last 90 days\.The default end time is the time of the event that occurred closest to the current time\.

### Valid *<timestamp>* formats<a name="w3ab1b7c11c15c23c12"></a>

The `--start-time` and `--end-time` attributes take UNIX time values or valid equivalents\.

The following are examples of valid formats\. Date, month, and year values can be separated by hyphens or forward slashes\. Double quotes must be used if spaces are present\.

```
1422317782
1422317782.0
01-27-2015
01-27-2015,01:16PM
"01-27-2015, 01:16 PM"
"01/27/2015, 13:16"
2015-01-27
"2015-01-27, 01:16 PM"
```

## Looking up events by attribute<a name="look-up-events-by-attributes"></a>

To filter by an attribute, type the following command:

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=<attribute>,AttributeValue=<string>
```

You can specify only one attribute key/value pair for each lookup\-events command\. The following are values for `AttributeKey`\. Value names are case sensitive\.

+ EventId

+ EventName

+ EventSource

+ ResourceName

+ ResourceType

+ Username

For a list of the resource types that are supported for lookup\-events, see [Resource Types Supported by CloudTrail Event History](view-cloudtrail-events-supported-resource-types.md)\.

### Attribute lookup examples<a name="attribute-lookup-example"></a>

The following example command returns the event for the specified CloudTrail `EventId`\.

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventId,AttributeValue=b5cc8c40-12ba-4d08-a8d9-2bceb9a3e002
```

The following example command returns events in which the value of `EventName` is `RunInstances`\.

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=RunInstances
```

The following example command returns events in which the value of `EventSource` is `iam.amazonaws.com`\.

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventSource,AttributeValue=iam.amazonaws.com
```

The following example command returns events in which the value of `ResourceName` is `CloudTrail_CloudWatchLogs_Role`\.

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceName,AttributeValue=CloudTrail_CloudWatchLogs_Role
```

The following example command returns events in which the value of `ResourceType` is `AWS::S3::Bucket`\.

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceType,AttributeValue=AWS::S3::Bucket
```

The following example command returns events in which the value of `Username` is `root`\.

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=Username,AttributeValue=root
```

## Specifying the next page of results<a name="specify-next-page-of-lookup-results"></a>

To get the next page of results from a `lookup-events` command, type the following command:

```
aws cloudtrail lookup-events <same parameters as previous command> --next-token=<token>
```

where the value for *<token>* is taken from the first field of the output of the previous command\. 

When you use `--next-token` in a command, you must use the same parameters as in the previous command\. For example, suppose you run the following command:

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=Username,AttributeValue=root
```

To get the next page of results, your next command would look like this:

```
aws cloudtrail lookup-events --lookup-attributes AttributeKey=Username,AttributeValue=root --next-token=kbOt5LlZe++mErCebpy2TgaMgmDvF1kYGFcH64JSjIbZFjsuvrSqg66b5YGssKutDYIyII4lrP4IDbeQdiObkp9YAlju3oXd12juy3CIZW8=
```

## Getting JSON input from a file<a name="json-input-from-file"></a>

The AWS CLI for some AWS services has two parameters, `--generate-cli-skeleton` and `--cli-input-json`, that you can use to generate a JSON template which you can modify and use as input to the `--cli-input-json` parameter\. This section describes how to use these parameters with `aws cloudtrail lookup-events`\. For more general information, see [ Generate CLI Skeleton and CLI Input JSON Parameters](http://docs.aws.amazon.com/cli/latest/userguide/generate-cli-skeleton.html)\.

**To look up CloudTrail events by getting JSON input from a file**

1. Create an input template for use with `lookup-events` by redirecting the `--generate-cli-skeleton` output to a file, as in the following example\.

   ```
   aws cloudtrail lookup-events --generate-cli-skeleton > LookupEvents.txt
   ```

   The template file generated \(in this case, LookupEvents\.txt\) looks like this:

   ```
   {
       "LookupAttributes": [
           {
               "AttributeKey": "",
               "AttributeValue": ""
           }
       ],
       "StartTime": null,
       "EndTime": null,
       "MaxResults": 0,
       "NextToken": ""
   }
   ```

1. Use a text editor to modify the JSON as needed\. The JSON input must contain only values that are specified\. 
**Important**  
All empty or null values must be removed from the template before you can use it\.

   The following example specifies a time range and maximum number of results to return\.

   ```
   {
       "StartTime": "2015-01-01",
       "EndTime": "2015-01-27",
       "MaxResults": 2
   }
   ```

1. To use the edited file as input, use the syntax `--cli-input-json file://`*<filename>*, as in the following example:

   ```
   aws cloudtrail lookup-events --cli-input-json file://LookupEvents.txt
   ```

**Note**  
You can use other arguments on the same command line as `--cli-input-json` \. 

## Lookup Output Fields<a name="view-cloudtrail-events-cli-output-fields"></a>

**Events**  
A list of lookup events based on the lookup attribute and time range that were specified\. The events list is sorted by time, with the latest event listed first\. Each entry contains information about the lookup request and includes a string representation of the CloudTrail event that was retrieved\.   
 The following entries describe the fields in each lookup event\. 

**CloudTrailEvent**  
A JSON string that contains an object representation of the event returned\. For information about each of the elements returned, see [ Record Body Contents](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)\. 

**EventId**  
A string that contains the GUID of the event returned\.

**EventName**  
A string that contains the name of the event returned\. 

**EventSource**  
The AWS service that the request was made to\. 

**EventTime**  
The date and time, in UNIX time format, of the event\. 

**Resources**  
A list of resources referenced by the event that was returned\. Each resource entry specifies a resource type and a resource name\. 

**ResourceName**  
A string that contains the name of the resource referenced by the event\. 

**ResourceType**  
A string that contains the type of a resource referenced by the event\. When the resource type cannot be determined, null is returned\. 

**Username**  
A string that contains the user name of the account for the event returned\. 

**NextToken**  
A string to get the next page of results from a previous `lookup-events` command\. To use the token, the parameters must be the same as those in the original command\. If no `NextToken` entry appears in the output, there are no more results to return\. 