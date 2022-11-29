# Event copy details<a name="copy-trail-details"></a>

After a trail event copy starts, you can view the event copy details, including the status of the copy, and information on any copy failures\.

**Note**  
Details shown on the event copy details page are not in real\-time\. The actual values for details such as **Prefixes copied** may be higher than what is shown on the page\. CloudTrail updates the details incrementally over the course of the event copy\.

**To access the event copy details page**

1. Sign in to the AWS Management Console and open the CloudTrail console at [https://console\.aws\.amazon\.com/cloudtrail/](https://console.aws.amazon.com/cloudtrail/)\.

1. Choose **Lake** in the left navigation pane of the CloudTrail console\.

1. On the **Lake** page, choose the **Event data stores** tab\.

1. Choose the event data store\.

1. Choose the event copy in the **Event copy status** section\.

## Copy details<a name="copy-trail-status"></a>

From **Copy details**, you can view the following details about the trail event copy\.
+ **Event log S3 location** \- The location of the source S3 bucket containing the trail event log files\.
+ **Copy ID** \- The ID for the copy\.
+ **Prefixes copied** \- Represents the number of S3 prefixes copied\. During a trail event copy, CloudTrail copies the events in the trail log files that are stored in the prefixes\.
+ **Copy status** \- The status of the copy\.
  + **Initializing** \- Initial status shown when the trail event copy starts\.
  + **In progress** \- Indicates the trail event copy is in progress\.
**Note**  
You cannot copy trail events if another trail event copy is **In progress**\. To stop a trail event copy, choose **Stop copy**\.
  + **Stopped** \- Indicates a **Stop copy** action occurred\. To retry a trail event copy, choose **Retry copy**\.
  + **Failed** \- The copy completed, but some trail events failed to copy\. Review the error messages in **Copy failures**\. To retry a trail event copy, choose **Retry copy**\. When you retry a copy, CloudTrail resumes the copy at the location where the failure occurred\.
  + **Completed** \- The copy completed without errors\. You can query the copied trail events in the event data store\.
+ **Created time** \- Indicates when the trail event copy started\.
+ **Finish time** \- Indicates when the trail event copy completed or stopped\.

## Copy failures<a name="copy-trail-failures"></a>

 From **Copy failures**, you can review the error location, error message, and error type for each copy failure\. Common reasons for failure, include if an S3 prefix contained an uncompressed file, or contained a file delivered by a service other than CloudTrail\. Another possible cause of failure relates to access issues\. For example, if the event data store's S3 bucket did not grant CloudTrail access to import the events, you would get an `AccessDenied` error\.

For each copy failure, review the following error information\.
+  The **Error location** \- Indicates the location in the S3 bucket where the error occurred\. If an error occurred because the source S3 bucket contained an uncompressed file, the **Error location** would include the prefix where you would find that file\. 
+  The **Error message** \- Provides an explanation for why the error occurred\. 
+  The **Error type** \- Provides the error type\. For example, an **Error type** of `AccessDenied`, indicates that the error occurred because of a permissions issue\. For more information about the required permissions for copying trail events, see [Required permissions for copying trail events](cloudtrail-copy-trail-to-lake-eds.md#copy-trail-events-permissions)\. 

After resolving any failures, choose **Retry copy**\. When you retry a copy, CloudTrail resumes the copy at the location where the failure occurred\. 