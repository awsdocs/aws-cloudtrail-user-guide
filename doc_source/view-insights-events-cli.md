# Viewing CloudTrail Insights Events with the AWS CLI<a name="view-insights-events-cli"></a>

You can look up CloudTrail Insights events for the last 90 days by running the aws cloudtrail lookup\-events command\. `lookup-events` has the following options:
+ `--end-time`
+ `--event-category`
+ `--max-results`
+ `--start-time`
+ `--lookup-attributes`
+ `--next-token`
+ `--generate-cli-skeleton`
+ `--cli-input-json`

These options are explained in this topic\. For general information about using the AWS Command Line Interface, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. 

**Contents**
+ [Prerequisites](#aws-cli-prerequisites-for-insights)
+ [Getting Command Line Help](#getting-command-line-help-insights)
+ [Looking Up Insights Events](#looking-up-insights-with-the-aws-cli)
+ [Specifying the Number of Insights Events to Return](#specify-the-number-of-insights-to-return)
+ [Looking Up Insights Events by Time Range](#look-up-insights-by-time-range)
  + [Valid *<timestamp>* Formats](#look-up-insights-by-time-range-formats)
+ [Looking Up Insights Events by Attribute](#look-up-insights-by-attributes)
  + [Attribute Lookup Examples](#attribute-lookup-example-insights)
+ [Specifying the Next Page of Results](#specify-next-page-of-results)
+ [Getting JSON Input from a File](#json-input-from-file-insights)
+ [Lookup Output Fields](#view-insights-events-cli-output-fields)

## Prerequisites<a name="aws-cli-prerequisites-for-insights"></a>
+ To run AWS CLI commands, you must install the AWS CLI\. For more information, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ Make sure your AWS CLI version is greater than 1\.6\.6\. To verify the CLI version, run aws \-\-version on the command line\.
+ To set the account, region, and default output format for an AWS CLI session, use the aws configure command\. For more information, see [ Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

**Note**  
The CloudTrail AWS CLI commands are case\-sensitive\.

## Getting Command Line Help<a name="getting-command-line-help-insights"></a>

To see the command line help for `lookup-events`, type the following command:

```
aws cloudtrail lookup-events help
```

## Looking Up Insights Events<a name="looking-up-insights-with-the-aws-cli"></a>

To see the ten latest Insights events, type the following command:

```
aws cloudtrail lookup-events --event-category insight
```

 A returned event looks similar to the following example, which has been formatted for readability:

```
{
    "NextToken": "kbOt5LlZe++mErCebpy2TgaMgmDvF1kYGFcH64JSjIbZFjsuvrSqg66b5YGssKutDYIyII4lrP4IDbeQdiObkp9YAlju3oXd12juEXAMPLE=", 
    "Events": [
        {
            "eventVersion": "1.07",
            "eventTime": "2019-10-15T21:13:00Z",
            "awsRegion": "us-east-1",
            "eventID": "EXAMPLE-9b6f-45f8-bc6b-9b41c052ebc7",
            "eventType": "AwsCloudTrailInsight",
            "recipientAccountId": "123456789012",
            "sharedEventID": "EXAMPLE8-02b2-4e93-9aab-08ed47ea5fd3",
            "insightDetails": {
                "state": "Start",
                "eventSource": "autoscaling.amazonaws.com",
                "eventName": "PutLifecycleHook",
                "insightType": "ApiCallRateInsight",
                "insightContext": {
                    "statistics": {
                        "baseline": {
                            "average": 0.0017857143
                        },
                        "insight": {
                            "average": 4
                        }
                    }
                }
            },
            "eventCategory": "Insight"
        },
        {
            "eventVersion": "1.07",
            "eventTime": "2019-10-15T21:14:00Z",
            "awsRegion": "us-east-1",
            "eventID": "EXAMPLEc-9eac-4af6-8e07-26a5ae8786a5",
            "eventType": "AwsCloudTrailInsight",
            "recipientAccountId": "123456789012",
            "sharedEventID": "EXAMPLE8-02b2-4e93-9aab-08ed47ea5fd3",
            "insightDetails": {
                "state": "End",
                "eventSource": "autoscaling.amazonaws.com",
                "eventName": "PutLifecycleHook",
                "insightType": "ApiCallRateInsight",
                "insightContext": {
                    "statistics": {
                        "baseline": {
                            "average": 0.0017857143
                        },
                        "insight": {
                            "average": 4
                        },
                        "insightDuration": 1
                    }
                }
            },
            "eventCategory": "Insight"
        }
    ]
}
```

For an explanation of the lookup\-related fields in the output, see the section [Lookup Output Fields](#view-insights-events-cli-output-fields) later in this document\. For an explanation of the fields in the Insights event, see [CloudTrail Record Contents](cloudtrail-event-reference-record-contents.md)\.

## Specifying the Number of Insights Events to Return<a name="specify-the-number-of-insights-to-return"></a>

To specify the number of events to return, type the following command:

```
aws cloudtrail lookup-events --event-category insight --max-results <integer>
```

The default value for *<integer>*, if it is not specified, is 10\. Possible values are 1 through 50\. The following example returns one result\.

```
aws cloudtrail lookup-events --event-category insight --max-results 1
```

## Looking Up Insights Events by Time Range<a name="look-up-insights-by-time-range"></a>

Insights events from the past 90 days are available for lookup\. To specify a time range, type the following command:

```
aws cloudtrail lookup-events --event-category insight --start-time <timestamp> --end-time <timestamp>
```

`--start-time <timestamp>` specifies that only Insights events that occur after or at the specified time are returned\. If the specified start time is after the specified end time, an error is returned\.

`--end-time <timestamp>` specifies that only Insights events that occur before or at the specified time are returned\. If the specified end time is before the specified start time, an error is returned\.

The default start time is the earliest date that data is available within the last 90 days\.The default end time is the time of the event that occurred closest to the current time\.

### Valid *<timestamp>* Formats<a name="look-up-insights-by-time-range-formats"></a>

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

## Looking Up Insights Events by Attribute<a name="look-up-insights-by-attributes"></a>

To filter by an attribute, type the following command:

```
aws cloudtrail lookup-events --event-category insight --lookup-attributes AttributeKey=<attribute>,AttributeValue=<string>
```

You can specify only one attribute key/value pair for each lookup\-events command\. The following are valid Insights event values for `AttributeKey`\. Value names are case sensitive\.
+ EventId
+ EventName
+ EventSource

### Attribute Lookup Examples<a name="attribute-lookup-example-insights"></a>

The following example command returns Insights events in which the value of `EventName` is `PutRule`\.

```
aws cloudtrail lookup-events --event-category insight --lookup-attributes AttributeKey=EventName, AttributeValue=PutRule
```

The following example command returns Insights events in which the value of `EventId` is `b5cc8c40-12ba-4d08-a8d9-2bceb9a3e002`\.

```
aws cloudtrail lookup-events --event-category insight --lookup-attributes AttributeKey=EventId, AttributeValue=b5cc8c40-12ba-4d08-a8d9-2bceb9a3e002
```

The following example command returns Insights events in which the value of `EventSource` is `iam.amazonaws.com`\.

```
aws cloudtrail lookup-events --event-category insight --lookup-attributes AttributeKey=EventSource, AttributeValue=iam.amazonaws.com
```

## Specifying the Next Page of Results<a name="specify-next-page-of-results"></a>

To get the next page of results from a `lookup-events` command, type the following command:

```
aws cloudtrail lookup-events --event-category insight <same parameters as previous command> --next-token=<token>
```

where the value for *<token>* is taken from the first field of the output of the previous command\.

When you use `--next-token` in a command, you must use the same parameters as in the previous command\. For example, suppose you run the following command:

```
aws cloudtrail lookup-events --event-category insight --lookup-attributes AttributeKey=EventName, AttributeValue=PutRule
```

To get the next page of results, your next command would look like this:

```
aws cloudtrail lookup-events --event-category insight --lookup-attributes AttributeKey=EventName,AttributeValue=PutRule --next-token=EXAMPLEZe++mErCebpy2TgaMgmDvF1kYGFcH64JSjIbZFjsuvrSqg66b5YGssKutDYIyII4lrP4IDbeQdiObkp9YAlju3oXd12juEXAMPLE=
```

## Getting JSON Input from a File<a name="json-input-from-file-insights"></a>

The AWS CLI for some AWS services has two parameters, `--generate-cli-skeleton` and `--cli-input-json`, that you can use to generate a JSON template which you can modify and use as input to the `--cli-input-json` parameter\. This section describes how to use these parameters with `aws cloudtrail lookup-events`\. For more general information, see [ Generate CLI Skeleton and CLI Input JSON Parameters](https://docs.aws.amazon.com/cli/latest/userguide/generate-cli-skeleton.html)\.

**To look up Insights events by getting JSON input from a file**

1. Create an input template for use with `lookup-events` by redirecting the `--generate-cli-skeleton` output to a file, as in the following example\.

   ```
   aws cloudtrail lookup-events --event-category insight --generate-cli-skeleton > LookupEvents.txt
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
   aws cloudtrail lookup-events --event-category insight --cli-input-json file://LookupEvents.txt
   ```

**Note**  
You can use other arguments on the same command line as `--cli-input-json`\.

## Lookup Output Fields<a name="view-insights-events-cli-output-fields"></a>

**Events**  
A list of lookup events based on the lookup attribute and time range that were specified\. The events list is sorted by time, with the latest event listed first\. Each entry contains information about the lookup request and includes a string representation of the CloudTrail event that was retrieved\.   
 The following entries describe the fields in each lookup event\.

**CloudTrailEvent**  
A JSON string that contains an object representation of the event returned\. For information about each of the elements returned, see [ Record Body Contents](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html)\. 

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

For more information about CloudTrail Insights events, see [Logging Insights Events for Trails](logging-insights-events-with-cloudtrail.md) in this guide\.