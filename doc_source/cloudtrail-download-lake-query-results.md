# Download your CloudTrail Lake saved query results<a name="cloudtrail-download-lake-query-results"></a>

When you save query results, CloudTrail delivers two types of files to your Amazon S3 bucket\.
+ A sign file in JSON format that you can use to validate the query result files\. The sign file is named result\_sign\.json\. For more information about the sign file, see [CloudTrail sign file structure](cloudtrail-results-file-validation-sign-file-structure.md)\.
+ One or more query result files in CSV format, which contain the results from the query\. The number of query result files delivered is dependent upon the total size of the query results\. The maximum file size for a query result file is 1 TB\. Each query result file is named result\_*number*\.csv\.gz\. For example, if the total size of the query results was 2 TB, you would have two query result files, result\_1\.csv\.gz and result\_2\.csv\.gz\.

 CloudTrail query result and sign files are Amazon S3 objects\. You can use the S3 console, the AWS Command Line Interface \(CLI\), or the S3 API to retrieve query result and sign files\. 

 The following procedure describes how to download the query result and sign files with the Amazon S3 console\. 

**To download your query result or sign file with the Amazon S3 console**

1. Open the Amazon S3 console\.

1. Choose the bucket and choose the file that you want to download\.  
![\[CloudTrail query result file\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/images/lake_query_results_S3.png)

1. Choose **Download** and follow any prompts to save the file\.
**Note**  
Some browsers, such as Chrome, automatically extract the query result file for you\. If your browser does this for you, skip to step 5\.

1. Use a product such as [7\-Zip](http://www.7-zip.org) to extract the query result file\.

1. Open the query result or sign file\.