# Run a query<a name="query-run-query"></a>

After you choose or save a query, you can run a query on an event data store\.

1. On the **Saved queries** or **Sample queries** tabs, choose a query to run by choosing the value in the **Query SQL** column\. In this walkthrough, on the **Saved queries** tab, choose the query that you created in [Create or edit a query](query-create-edit-query.md)\.

1. On the **Editor** tab, for **Event data store**, choose an event data store from the drop\-down list\.

1. On the **Editor** tab, choose **Run**\.

   Depending on the size of your event data store, and the number of days of data it includes, a query can take several minutes to run\. The **Command output** tab shows the status of a query, and whether a query is finished running\. When a query has finished running, open the **Query results** tab to see a table of results for the active query \(the query currently shown in the editor\)\.

**Note**  
Queries that run for longer than one hour might time out\. You can still get partial results that were processed before the query timed out\.