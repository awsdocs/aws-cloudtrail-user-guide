# Create or edit a query<a name="query-create-edit-query"></a>

Queries in CloudTrail are authored in SQL\. You can build a query on the CloudTrail Lake **Editor** tab by writing the query in SQL from scratch, or by opening a saved or sample query and editing it\. You cannot overwrite an included sample query with your changes, but you can save it as a new query\. For more information about the SQL query language that is allowed, see [CloudTrail Lake SQL constraints](query-limitations.md)\.

An unbounded query \(such as `SELECT * FROM edsID`\) scans all data in your event data store\. To help control costs, we recommend that you constrain queries by adding starting and ending `eventTime` time stamps to queries\. The following is an example that searches for all events in a specified event data store where the event time is after \(`>`\) January 5, 2022 at 1:51 p\.m\. and before \(`<`\) January 19, 2022 at 1:51 p\.m\. Because an event data store has a minimum retention period of seven days, the minimum time span between starting and ending `eventTime` values is also seven days\.

```
SELECT *
FROM eds-ID
WHERE
     eventtime >='2022-01-05 13:51:00' and eventtime < ='2022-01-19 13:51:00'
```

In this walkthrough, we open one of the sample queries, edit it to select events that have a `userIdentity.principalId` value like `Alice` and an event name of `ConsoleLogin`, and save it as a new query\. You can also edit a saved query on the **Saved queries** tab, if you have saved queries\.

1. On the CloudTrail **Lake** page, open the **Sample queries** tab\.

1. Open **sample query 1** by choosing the **Query SQL** string for the query\. This opens the query in the **Editor** tab\.

1. In the **Editor** tab, add lines for `FROM`, `WHERE`, and `GROUP BY` as shown in the following example\. The value of `FROM` is the ID portion of the event data store's ARN\. The event data store ID from the ARN is displayed in the **Event data store** pane at left when you have the event data store that you want selected in the drop\-down list\. Choose **Copy** to copy the ID to the clipboard, so you can paste it into your query SQL\.

   ```
   SELECT
        userIdentity.principalId, count(*)
   FROM
        5e2676f8-9fce-46ef-8142-3e36a94e6691
   WHERE
        userIdentity.principalId LIKE '%Alice%'
        AND eventName = 'ConsoleLogin'
   GROUP BY
        userIdentity.principalId
   ```

1. You can run a query before you save it, to verify that the query works\. To run a query, choose an event data store from the **Event data store** drop\-down list, and then choose **Run**\. View the **Status** column of the **Command output** tab for the active query to verify that a query ran successfully\.

1. When you have added the lines to the sample query, choose **Save**\.

1. In **Save query**, enter a name and description for the query\. Choose **Save query** to save your changes as the new query\. To discard changes to a query, choose **Cancel**, or close the **Save query** window\.  
![\[Saving a changed query\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-save.png)
**Note**  
Saved queries are tied to your browser; if you use a different browser or a different device to access the CloudTrail console, the saved queries are not available\.

1. Open the **Saved queries** tab to see the new query in the table\.  
![\[Saved queries tab\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-saved-table.png)

## Query editor tools<a name="query-editor-format-controls"></a>

A toolbar at the upper right of the query editor offers commands to help author and format your SQL query\.

![\[Query editor toolbar\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/query-editor-toolbar.png)

The following list describes the commands on the toolbar\.
+ **Undo** \- Reverts the last content change made in the query editor\.
+ **Redo** \- Repeats the last content change made in the query editor\.
+ **Format selected** \- Arranges the query editor content according to SQL formatting and spacing conventions\.