# Example queries<a name="query-lake-examples"></a>

This section includes example CloudTrail Lake queries to help you get started\.

**Topics**
+ [Find all principal user identities who called `CreateBucket` on January 22, 2022](#query-example-principal-ids)
+ [Find all APIs that a user called on January 22, 2022](#query-example-user-called-apis)
+ [Find the number of API calls since January 1, 2022, grouped by `eventName` and `eventSource`](#query-example-apis-eventname-eventsource)
+ [Find all users who signed in to the console in a set of regions](#query-example-users-login-regions)
+ [Find all CloudTrail Lake queries that were run in January 2022](#query-example-all-queries)

## Find all principal user identities who called `CreateBucket` on January 22, 2022<a name="query-example-principal-ids"></a>

```
SELECT 
    userIdentity.principalid, 
    eventName 
FROM 
    event_data_store_ID
WHERE 
    userIdentity.principalid IS NOT NULL 
AND 
    eventTime > '2022-01-22 00:00:00' 
AND 
    eventTime < '2022-01-23 00:00:00' 
AND 
    eventName='CreateBucket'
```

Results

```
{
    "QueryStatus": "FINISHED",
    "QueryStatistics": {
        "ResultsCount": 1,
        "TotalResultsCount": 1,
        "BytesScanned": 25077
    },
    "QueryResultRows": [
        [
            {
                "principalid": "principal_ID"
            },
            {
                "eventName": "CreateBucket"
            }
        ]
    ]
}
```

## Find all APIs that a user called on January 22, 2022<a name="query-example-user-called-apis"></a>

```
SELECT 
    eventID, 
    eventName, 
    eventSource, 
    eventTime
FROM 
    event_data_store_ID
WHERE 
    userIdentity.username = 'bob' 
AND 
    eventTime > '2022-01-22 00:00:00' 
AND 
    eventTime < '2022-01-23 00:00:00'
```

Results

```
{
    "QueryStatus": "FINISHED",
    "QueryStatistics": {
        "ResultsCount": 2,
        "TotalResultsCount": 2,
        "BytesScanned": 13287
    },
    "QueryResultRows": [
        [
            {
                "eventID": "EXAMPLE-c3b6-43e4-aa35-b2490EXAMPLE"
            },
            {
                "eventName": "DescribeQuery"
            },
            {
                "eventSource": "cloudtrail.amazonaws.com"
            },
            {
                "eventTime": "2022-01-22 16:53:53.000"
            }
        ],
        [
            {
                "eventID": "EXAMPLE6-ac95-4b37-b587-76a80EXAMPLE"
            },
            {
                "eventName": "ListBuckets"
            },
            {
                "eventSource": "s3.amazonaws.com"
            },
            {
                "eventTime": "2022-01-22 20:25:01.000"
            }
        ]
    ]
}
```

## Find the number of API calls since January 1, 2022, grouped by `eventName` and `eventSource`<a name="query-example-apis-eventname-eventsource"></a>

```
SELECT 
    eventSource, 
    eventName, 
    COUNT(*) AS apiCount
FROM 
    event_data_store_ID
WHERE 
    eventTime > '2022-01-01 00:00:00' 
GROUP BY 
    eventSource, eventName 
ORDER BY 
    apiCount DESC
```

Results

```
{
    "QueryStatus": "FINISHED",
    "QueryStatistics": {
        "ResultsCount": 3,
        "TotalResultsCount": 3,
        "BytesScanned": 10442
    },
    "QueryResultRows": [
        [
            {
                "eventSource": "s3.amazonaws.com"
            },
            {
                "eventName": "PutObject"
            },
            {
                "apiCount": "96059"
            }
        ],
        [
            {
                "eventSource": "dynamodb.amazonaws.com"
            },
            {
                "eventName": "DescribeTable"
            },
            {
                "apiCount": "49426"
            }
        ],
        [
            {
                "eventSource": "sts.amazonaws.com"
            },
            {
                "eventName": "AssumeRole"
            },
            {
                "apiCount": "45617"
            }
        ]
    ]
}
```

## Find all users who signed in to the console in a set of regions<a name="query-example-users-login-regions"></a>

```
SELECT 
    eventTime, 
    useridentity.arn, 
    awsRegion
FROM 
    event_data_store_ID
WHERE 
    awsRegion in ('us-east-1', 'us-west-2') 
AND
    eventName = 'ConsoleLogin'
```

Results

```
{
    "QueryStatus": "FINISHED",
    "QueryStatistics": {
        "ResultsCount": 2,
        "TotalResultsCount": 2,
        "BytesScanned": 15580
    },
    "QueryResultRows": [
        [
            {
                "eventTime": "2022-02-08 19:54:44.000"
            },
            {
                "arn": "arn:aws:sts::123456789012:assumed-role/example-identity"
            },
            {
                "awsRegion": "us-east-1"
            }
        ],
        [
            {
                "eventTime": "2022-01-21 16:38:27.000"
            },
            {
                "arn": "arn:aws:sts::123456789012:assumed-role/example-identity"
            },
            {
                "awsRegion": "us-west-2"
            }
        ]
    ]
}
```

## Find all CloudTrail Lake queries that were run in January 2022<a name="query-example-all-queries"></a>

```
SELECT 
    element_at(responseElements, 'queryId'), 
    element_at(requestParameters, 'queryStatement')
FROM 
    event_data_store_ID
WHERE 
    eventName='StartQuery' 
AND 
    eventSource = 'cloudtrail.amazonaws.com' 
AND 
    responseElements IS NOT NULL
AND 
    eventTime > '2022-01-01 00:00:00' 
AND 
    eventTime < '2022-02-01 00:00:00'
```

Results

```
{
    "QueryStatus": "FINISHED",
    "QueryStatistics": {
        "ResultsCount": 1,
        "TotalResultsCount": 1,
        "BytesScanned": 13002
    },
    "QueryResultRows": [
        [
            {
                "_col0": "arn:aws:cloudtrail:us-east-1:123456789012:eventdatastore/EXAMPLE-f852-4e8f-8bd1-bcf6cEXAMPLE"
            },
            {
                "_col1": "select * from event_data_store_ID limit 1;"
            }
        ]
    ]
}
```