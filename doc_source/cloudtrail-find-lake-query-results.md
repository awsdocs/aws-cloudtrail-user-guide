# Find your CloudTrail Lake saved query results<a name="cloudtrail-find-lake-query-results"></a>

CloudTrail publishes query result and sign files to your S3 bucket\. The query result file contains the output of the saved query and the sign file provides the signature and hash value for the query results\. You can use the sign file to validate the query results\. For more information about validating query results, see [Validate saved query results](cloudtrail-query-results-validation-intro.md)\.

To retrieve a query result or sign file, you can use the Amazon S3 console, the Amazon S3 command line interface \(CLI\), or the API\. 

**To find your query results and sign files with the Amazon S3 console**

1. Open the Amazon S3 console\.

1. Choose the bucket you specified\.

1. Navigate through the object hierarchy until you find the query result and sign files\. The query result file has a \.csv\.gz extension and the sign file has a \.json extension\.

You will navigate through an object hierarchy that is similar to the following example, but with a different bucket name, account ID, date, and query ID\. 

```
All Buckets
    Bucket_Name
        AWSLogs
            Account_ID;
                CloudTrail-Lake
                    Query
                        2022
                            06
                              20
                                Query_ID
```