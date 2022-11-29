# View query results<a name="query-results"></a>

After your query finishes, you can view its results\. The results of a query are available for seven days after the query finishes\. You can view results for the active query on the **Query results** tab, or you can access results for all recent queries on the **Results history** tab on the **Lake** home page\.

Query results can change from older runs of a query to newer ones, as later events in the query period can be logged between queries\.

When you save query results, the query results may display in the CloudTrail console before they are viewable in the S3 bucket since CloudTrail delivers the query results after the query scan completes\. While most queries complete within a few minutes, depending on the size of your event data store, it can take considerably longer for CloudTrail to deliver query results to your S3 bucket\. CloudTrail delivers the query results to the S3 bucket in compressed gzip format\.  On average, after the query scan completes you can expect a latency of 6 minutes for every GB of data delivered to the S3 bucket\. For more information about finding and downloading saved query results, see [Get and download saved query results](view-download-cloudtrail-lake-query-results.md)\.

**Note**  
Queries that run for longer than one hour might time out\. You can still get partial results that were processed before the query timed out\. CloudTrail does not deliver partial query results to an S3 bucket\. To avoid a time out, you can refine your query to limit the amount of data scanned by specifying a narrower time range\.

1. On the **Query results** tab for an active query, each row represents an event result that matched the query\. Filter results by entering all or part of an event field value in the search bar\.  
![\[Query results\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-results.png)

1. On the **Command output** tab, view metadata about the query that was run, such as the event data store ID, run time, number of results scanned, and whether or not the query was successful\. If you saved the query results to an Amazon S3 bucket, the metadata also includes a link to the S3 bucket containing the saved query results\.  
![\[Query command output\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-command-output.png)