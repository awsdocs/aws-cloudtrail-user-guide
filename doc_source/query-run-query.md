# Run a query and save query results<a name="query-run-query"></a>

After you choose or save a query, you can run a query on an event data store\. 

 When you run a query, you have the option to save the query results to an Amazon S3 bucket\. When you run queries in CloudTrail Lake, you incur charges based on the amount of data scanned by the query\. There are no additional CloudTrail Lake charges for saving query results to an S3 bucket, however, there are S3 storage charges\. For more information about S3 pricing, see [Amazon S3 pricing](http://aws.amazon.com/s3/pricing/)\. 

 When you save query results, the query results may display in the CloudTrail console before they are viewable in the S3 bucket since CloudTrail delivers the query results after the query scan completes\. While most queries complete within a few minutes, depending on the size of your event data store, it can take considerably longer for CloudTrail to deliver query results to your S3 bucket\. CloudTrail delivers the query results to the S3 bucket in compressed gzip format\.  On average, after the query scan completes you can expect a latency of 6 minutes for every GB of data delivered to the S3 bucket\. 

**To run a query using CloudTrail Lake**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1.  From the navigation pane, open the **Lake** submenu, then choose **Query**\. 

1. On the **Saved queries** or **Sample queries** tabs, choose a query to run by choosing the value in the **Query SQL** column\. 

1. On the **Editor** tab, for **Event data store**, choose an event data store from the drop\-down list\.

1. \(Optional\) On the **Editor** tab, choose **Save results to S3** to save the query results to an S3 bucket\. When you choose the default S3 bucket, CloudTrail creates and applies the required bucket policies\. For more information about saving query results, see [Additional information about saved query results](save-query-results.md)\. 
**Note**  
 To use a different bucket, specify a bucket name, or choose **Browse S3** to choose a bucket\. The bucket policy must grant CloudTrail permission to deliver query results to the bucket\. For information about manually editing the bucket policy, see [Amazon S3 bucket policy for CloudTrail Lake query results](s3-bucket-policy-lake-query-results.md)\. 

1. On the **Editor** tab, choose **Run**\.

   Depending on the size of your event data store, and the number of days of data it includes, a query can take several minutes to run\. The **Command output** tab shows the status of a query, and whether a query is finished running\. When a query has finished running, open the **Query results** tab to see a table of results for the active query \(the query currently shown in the editor\)\.

**Note**  
Queries that run for longer than one hour might time out\. You can still get partial results that were processed before the query timed out\. CloudTrail does not deliver partial query results to an S3 bucket\. To avoid a time out, you can refine your query to limit the amount of data scanned by specifying a narrower time range\.