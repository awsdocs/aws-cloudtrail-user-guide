# View query results<a name="query-results"></a>

After your query finishes, you can view its results\. The results are logged management or data events that matched your query criteria\. The results of a query are available for seven days after the query finishes\. You can view results for the active query on the **Query results** tab, or you can access results for all recent queries on the **Results history** tab on the **Lake** home page\.

Query results can change from older runs of a query to newer ones, as later events in the query period can be logged between queries\.

1. On the **Query results** tab for an active query, each row represents an event result that matched the query\. Filter results by entering all or part of an event field value in the search bar\.  
![\[Query results\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-results.png)

1. On the **Command output** tab, view metadata about the query that was run, such as the event data store ID, run time, number of results scanned, and whether or not the query was successful\.  
![\[Query command output\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-command-output.png)