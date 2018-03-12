# Finding Your CloudTrail Log Files<a name="cloudtrail-find-log-files"></a>

CloudTrail publishes log files to your S3 bucket in a gzip archive\. In the S3 bucket, the log file has a formatted name that includes the following elements: 

+ The bucket name that you specified when you created trail \(found on the Trails page of the CloudTrail console\)

+ The \(optional\) prefix you specified when you created your trail

+ The string "AWSLogs"

+ The account number

+ The string "CloudTrail"

+ A region identifier such as us\-west\-1

+ The year the log file was published in `YYYY` format

+ The month the log file was published in `MM` format

+ The day the log file was published in `DD` format

+ An alphanumeric string that disambiguates the file from others that cover the same time period 

The following example shows a complete log file object name:

```
bucket_name/prefix_name/AWSLogs/Account ID/CloudTrail/region/YYYY/MM/DD/file_name.json.gz
```

To retrieve a log file, you can use the Amazon S3 console, the Amazon S3 command line interface \(CLI\), or the API\. 

**To find your log files with the Amazon S3 console**

1. Open the Amazon S3 console\.

1. Choose the bucket you specified\.

1. Navigate through the object hierarchy until you find the log file you want\.

   All log files have a \.gz extension\.

You will navigate through an object hierarchy that is similar to the following example, but with a different bucket name, account ID, region, and date\. 

```
All Buckets
    Bucket_Name
        AWSLogs
            123456789012
                CloudTrail
                    us-west-1
                        2014
                            06
                                20
```

 A log file for the preceding object hierarchy will look like the following: 

```
123456789012_CloudTrail_us-west-1_20140620T1255ZHdkvFTXOA3Vnhbc.json.gz
```

**Note**  
Although uncommon, you may receive log files that contain one or more duplicate events\. Duplicate events will have the same **eventID**\. For more information about the **eventID** field, see [CloudTrail Record Contents](cloudtrail-event-reference-record-contents.md)\.