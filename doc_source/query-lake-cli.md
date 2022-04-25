# Managing CloudTrail Lake by using the AWS CLI<a name="query-lake-cli"></a>

The following are example AWS CLI commands for creating and managing event data stores and queries in CloudTrail Lake\.

**Topics**
+ [Create an event data store](#lake-cli-create-eds)
+ [Get an event data store](#lake-cli-get-eds)
+ [List all event data stores in an account](#lake-cli-list-eds)
+ [Update an event data store](#lake-cli-update-eds)
+ [Delete an event data store](#lake-cli-delete-eds)
+ [Restore an event data store](#lake-cli-restore-eds)
+ [Start a query](#lake-cli-start-query)
+ [Get metadata about a query](#lake-cli-describe-query)
+ [Get query results](#lake-cli-get-query-results)
+ [List all queries on an event data store](#lake-cli-list-queries)
+ [Cancel a running query](#lake-cli-cancel-query)

## Create an event data store<a name="lake-cli-create-eds"></a>

The following example AWS CLI command creates an event data store named `my-event-data-store` that selects all Amazon S3 data events\. The event data store retention period in this example is 90 days\.`--name` is required; other parameters are optional\. Valid values for `--retention-period` are integers between 7 and 2555, representing days\. If you do not specify `--retention-period`, CloudTrail uses the default retention period of 2555 days\. By default, `--multi-region-enabled` is set, even if the parameter is not added, and the event data store includes events from all regions\. The event data store is not enabled for all accounts in an AWS Organizations organization by default; to enable an event data store to collect events for all accounts in an organization, add the `--organization-enabled` parameter\. `--termination-protection-enabled` \(the default\) and `--no-termination-protection-enabled` set and remove termination protection, respectively\. `--advanced-event-selectors` includes or excludes data events in your event data store; for more information about `--advanced-event-selectors`, see [https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutEventSelectors.html#awscloudtrail-PutEventSelectors-request-AdvancedEventSelectors](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_PutEventSelectors.html#awscloudtrail-PutEventSelectors-request-AdvancedEventSelectors) in the CloudTrail API Reference\.

When you use advanced event selectors to select for data events on S3 objects, always use the `StartsWith` operator\.

```
aws cloudtrail create-event-data-store
--name my-event-data-store
--retention-period 90
--advanced-event-selectors '[
        {
            "Name": "Select all S3 data events",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
                { "Field": "resources.ARN", "StartsWith": ["arn:aws:s3"] }
            ]
        }
    ]'
```

The following is an example response\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "CREATED",
    "AdvancedEventSelectors": [
        {
            "Name": "Select all S3 data events",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3::Object"
                    ]
                },
                {
                    "Field": "resources.ARN",
                    "StartsWith": [
                        "arn:aws:s3"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 90,
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2021-12-09T22:19:39.417000-05:00",
    "UpdatedTimestamp": "2021-12-09T22:19:39.603000-05:00"
}
```

## Get an event data store<a name="lake-cli-get-eds"></a>

The following AWS CLI example returns information about the event data store specified by the required `--event-data-store` parameter, which accepts an ARN or the ID suffix of the ARN\.

```
aws cloudtrail get-event-data-store
--event-data-store arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
```

The following is an example response\. Creation and last updated times are in `timestamp` format\.

```
{
    "EventDataStoreARN": "arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "Enabled",
    "AdvancedEventSelectors": [
        {
            "Name": "Select All S3 Data Events",
            "FieldSelectors": [
                { "Field": "eventCategory", "Equals": ["Data"] },
                { "Field": "resources.type", "Equals": ["AWS::S3::Object"] },
                { "Field": "resources.ARN", "StartsWith": ["arn:aws:s3"] }
            ]
        }
    ]
    "CreatedTimestamp": 1248496624,
    "UpdatedTimestamp": 1598296624,
    "MultiRegionEnabled": true,
    "RetentionPeriod": 90,
    "TerminationProtectionEnabled": true,
}
```

## List all event data stores in an account<a name="lake-cli-list-eds"></a>

The following example AWS CLI command returns information about all event data stores in an account, in the current region\. Optional parameters include `--max-results`, to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail list-event-data-stores
```

The following is an example response\.

```
{
    "EventDataStores": [
        {
            "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
            "Name": "my-event-data-store"
        }
    ]
}
```

## Update an event data store<a name="lake-cli-update-eds"></a>

The following example updates an event data store to change its retention period to 100 days, and enable termination protection\. The required `--event-data-store` parameter value is an ARN \(or the ID suffix of the ARN\) and is required; other parameters are optional\. In this example, the `--retention-period` parameter is added to change the retention period to 100 days\. `--termination-protection-enabled` is added to enable termination protection on an event data store that did not have termination protection enabled\.

```
aws cloudtrail update-event-data-store
--event-data-store arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--retention-period 100
--termination-protection-enabled
```

The following is an example response\.

```
{
    "EventDataStoreArn": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-ee54-4813-92d5-999aeEXAMPLE",
    "Name": "my-event-data-store",
    "Status": "ENABLED",
    "AdvancedEventSelectors": [
        {
            "Name": "Select all S3 data events",
            "FieldSelectors": [
                {
                    "Field": "eventCategory",
                    "Equals": [
                        "Data"
                    ]
                },
                {
                    "Field": "resources.type",
                    "Equals": [
                        "AWS::S3::Object"
                    ]
                },
                {
                    "Field": "resources.ARN",
                    "StartsWith": [
                        "arn:aws:s3"
                    ]
                }
            ]
        }
    ],
    "MultiRegionEnabled": true,
    "OrganizationEnabled": false,
    "RetentionPeriod": 100,
    "TerminationProtectionEnabled": true,
    "CreatedTimestamp": "2021-12-09T22:19:39.417000-05:00",
    "UpdatedTimestamp": "2021-12-09T22:19:39.603000-05:00"
}
```

## Delete an event data store<a name="lake-cli-delete-eds"></a>

The following example AWS CLI command disables the event data store specified by `--event-data-store`, which accepts an event data store ARN, or the ID suffix of the ARN\. After you run delete\-event\-data\-store, the final state of the event data store is `PENDING_DELETION`, and the event data store is automatically deleted after a wait period of seven days\. `--no-termination-protection-enabled` must be set on the event data store; this operation cannot work if `--termination-protection-enabled` is set\.

After you run delete\-event\-data\-store on an event data store, you cannot run list\-queries, describe\-query, or get\-query\-results on queries that are using the disabled data store\. The event data store does not count towards your account maximum of five event data stores when it is pending deletion\.

```
aws cloudtrail delete-event-data-store
--event-data-store arn:aws:cloudtrail:us-east-1:12345678910:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
```

There is no response if the operation is successful\.

## Restore an event data store<a name="lake-cli-restore-eds"></a>

The following example AWS CLI command restores an event data store that is pending deletion\. The event data store is specified by `--event-data-store`, which accepts an event data store ARN or the ID suffix of the ARN\. You can only restore a deleted event data store within the seven\-day wait period after deletion\.

```
aws cloudtrail restore-event-data-store
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
```

The response includes information about the event data store, including its ARN, advanced event selectors, and the status of restoration\.

## Start a query<a name="lake-cli-start-query"></a>

The following example runs a query on the event data store specified as an ID in the query statement\. The required `--query-statement` parameter provides a SQL query, enclosed in single quotation marks\.

```
aws cloudtrail start-query
--query-statement 'SELECT eventID, eventTime FROM EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE LIMIT 10'
```

The response is a `QueryId` string\. To get the status of a query, run describe\-query using the `QueryId` value returned by start\-query\. If the query is successful, you can run get\-query\-results to get results\.

**Output**

```
{
    "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE"
}
```

**Note**  
Queries that run for longer than one hour might time out\. You can still get partial results that were processed before the query timed out\.

## Get metadata about a query<a name="lake-cli-describe-query"></a>

The following example AWS CLI describe\-query command gets metadata about a query, including query run time in milliseconds, number of events scanned and matched, total number of bytes scanned, and query status\. The `BytesScanned` value matches the number of bytes for which your account is billed for the query, unless the query is still running\.

You must specify an ARN or the ID suffix of an ARN for `--event-data-store`, and a value for `--query-id`\.

```
aws cloudtrail describe-query
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--query-id EXAMPLEd-17a7-47c3-a9a1-eccf7EXAMPLE
```

The following is an example response\.

```
{
    "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE", 
    "QueryString": "SELECT eventID, eventTime FROM EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE LIMIT 10", 
    "QueryStatus": "RUNNING",
    "QueryStatistics": {
        "EventsMatched": 10,
        "EventsScanned": 1000,
        "BytesScanned": 35059,
        "ExecutionTimeInMillis": 3821,
        "CreationTime": "1598911142"
    }
}
```

## Get query results<a name="lake-cli-get-query-results"></a>

The following example AWS CLI command gets event data results of a query\. You must specify an ARN or the ID suffix of the ARN for `--event-data-store`, and the `QueryID` returned by the start\-query command\. The `BytesScanned` value matches the number of bytes for which your account is billed for the query, unless the query is still running\. Optional parameters include `--max-query-results`, to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-query-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail get-query-results
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--query-id EXAMPLEd-17a7-47c3-a9a1-eccf7EXAMPLE
```

**Output**

```
{
    "QueryStatus": "RUNNING",
    "QueryStatistics": {
        "ResultsCount": 244,
        "TotalResultsCount": 1582,
        "BytesScanned":27044
    },
    "QueryResults": [
      {
        "key": "eventName",
        "value": "StartQuery",
      }
   ],
    "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE", 
    "QueryString": "SELECT eventID, eventTime FROM EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
 LIMIT 10",
    "NextToken": "20add42078135EXAMPLE"
}
```

## List all queries on an event data store<a name="lake-cli-list-queries"></a>

The following example AWS CLI command returns a list of queries and query statuses on a specified event data store for the past seven days\. You must specify an ARN or the ID suffix of an ARN value for `--event-data-store`\. Optionally, to shorten the list of results, you can specify a time range, formatted as timestamps, by adding `--start-time` and `--end-time` parameters, and a `--query-status` value\. Valid values for `QueryStatus` include `QUEUED`, `RUNNING`, `FINISHED`, `FAILED`, or `CANCELLED`\.

list\-queries also has optional pagination parameters\. Use `--max-results` to specify a maximum number of results that you want the command to return on a single page\. If there are more results than your specified `--max-results` value, run the command again adding the returned `NextToken` value to get the next page of results\.

```
aws cloudtrail list-queries
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--query-status CANCELLED
--start-time 1598384589
--end-time 1598384602
--max-results 10
```

**Output**

```
{
    "Queries": [
        {
          "QueryId": "EXAMPLE2-0add-4207-8135-2d8a4EXAMPLE", 
          "QueryStatus": "CANCELLED",
          "CreationTime": 1598911142
        },
        {
          "QueryId": "EXAMPLE2-4e89-9230-2127-5dr3aEXAMPLE", 
          "QueryStatus": "CANCELLED",
          "CreationTime": 1598296624"
        }
     ],
    "NextToken": "20add42078135EXAMPLE"
}
```

## Cancel a running query<a name="lake-cli-cancel-query"></a>

The following example cancels a query with a status of `RUNNING`\. You must specify an ARN or the ID suffix of an ARN value for `--event-data-store`, and a value for `--query-id`\. When you run cancel\-query, the query status might show as `CANCELLED` even if the cancel\-query operation is not yet finished\.

**Note**  
A canceled query can incur charges\. Your account is still charged for the amount of data that was scanned before you canceled the query\.

The following is a CLI example\.

```
aws cloudtrail cancel-query
--event-data-store EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE
--query-id EXAMPLEd-17a7-47c3-a9a1-eccf7EXAMPLE
```

**Output**

```
QueryId -> (string)
QueryStatus -> (string)
```