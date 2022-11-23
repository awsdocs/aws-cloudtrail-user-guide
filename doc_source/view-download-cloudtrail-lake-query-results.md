# Get and download saved query results<a name="view-download-cloudtrail-lake-query-results"></a>

After you save query results, you need to be able to locate the file containing the query results\. CloudTrail delivers your query results to an Amazon S3 bucket that you specify when you save the query results\. 

**Note**  
 When you save query results, the query results may display in the console before they are viewable in the S3 bucket since CloudTrail delivers the query results after the query scan completes\. While most queries complete within a few minutes, depending on the size of your event data store, it can take considerably longer for CloudTrail to deliver query results to your S3 bucket\. CloudTrail delivers the query results to the S3 bucket in compressed gzip format\. On average, after the query scan completes you can expect a latency of 6 minutes for every GB of data delivered to the S3 bucket\. 

**Topics**
+ [Find your CloudTrail Lake saved query results](cloudtrail-find-lake-query-results.md)
+ [Download your CloudTrail Lake saved query results](cloudtrail-download-lake-query-results.md)