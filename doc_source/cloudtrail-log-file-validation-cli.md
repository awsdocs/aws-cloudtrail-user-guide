# Validating CloudTrail Log File Integrity with the AWS CLI<a name="cloudtrail-log-file-validation-cli"></a>

To validate logs with the AWS Command Line Interface, use the CloudTrail `validate-logs` command\. The command uses the digest files delivered to your Amazon S3 bucket to perform the validation\. For information about digest files, see [CloudTrail Digest File Structure](cloudtrail-log-file-validation-digest-file-structure.md)\. 

The AWS CLI allows you to detect the following types of changes:

+ Modification or deletion of CloudTrail log files

+ Modification or deletion of CloudTrail digest files

+ Modification or deletion of both of the above

**Note**  
The AWS CLI validates only log files that are referenced by digest files\. For more information, see [Checking Whether a Particular File was Delivered by CloudTrail ](#cloudtrail-log-file-validation-cli-validate-logs-check-file)\.

## Prerequisites<a name="cloudtrail-log-file-validation-cli-prerequisites"></a>

To validate log file integrity with the AWS CLI, the following conditions must be met:

+ You must have online connectivity to AWS\.

+ You must have read access to the Amazon S3 bucket that contains the digest and log files\. 

+ The digest and log files must not have been moved from the original Amazon S3 location where CloudTrail delivered them\.

**Note**  
Log files that have been downloaded to local disk cannot be validated with the AWS CLI\. For guidance on creating your own tools for validation, see [Custom Implementations of CloudTrail Log File Integrity Validation ](cloudtrail-log-file-custom-validation.md)\.

## validate\-logs<a name="cloudtrail-log-file-validation-cli-validate-logs"></a>

### Syntax<a name="cloudtrail-log-file-validation-cli-validate-logs-syntax"></a>

The following is the syntax for `validate-logs`\. Optional parameters are shown in brackets\.

`aws cloudtrail validate-logs --trail-arn <trailARN> --start-time <start-time> [--end-time <end-time>] [--s3-bucket <bucket-name>] [--s3-prefix <prefix>] [--verbose]` 

### Options<a name="cloudtrail-log-file-validation-cli-validate-logs-options"></a>

The following are the command\-line options for `validate-logs`\. The `--trail-arn` and `--start-time` options are required\. 

`--start-time`  
Specifies that log files delivered on or after the specified UTC timestamp value will be validated\. Example: `2015-01-08T05:21:42Z`\. 

`--end-time`  
Optionally specifies that log files delivered on or before the specified UTC timestamp value will be validated\. The default value is the current UTC time \(`Date.now()`\)\. Example: `2015-01-08T12:31:41Z`\.   
For the time range specified, the `validate-logs` command checks only the log files that are referenced in their corresponding digest files\. No other log files in the Amazon S3 bucket are checked\. For more information, see [Checking Whether a Particular File was Delivered by CloudTrail ](#cloudtrail-log-file-validation-cli-validate-logs-check-file)\. 

`--bucket-name`  
Optionally specifies the Amazon S3 bucket where the digest files are stored\. If a bucket name is not specified, the AWS CLI will retrieve it by calling `DescribeTrails()`\. 

`--prefix`  
Optionally specifies the Amazon S3 prefix where the digest files are stored\. If not specified, the AWS CLI will retrieve it by calling `DescribeTrails()`\.   
You should use this option only if your current prefix is different from the prefix that was in use during the time range that you specify\.

`--trail-arn`  
Specifies the Amazon Resource Name \(ARN\) of the trail to be validated\. The format of a trail ARN follows\.  

```
arn:aws:cloudtrail:us-east-2:111111111111:trail/MyTrailName
```
To obtain the trail ARN for a trail, you can use the `describe-trails` command before running `validate-logs`\.  
You may want to specify the bucket name and prefix in addition to the trail ARN if log files have been delivered to more than one bucket in the time range that you specified, and you want to restrict the validation to the log files in only one of the buckets\. 

`--verbose`  
Optionally outputs validation information for every log or digest file in the specified time range\. The output indicates whether the file remains unchanged or has been modified or deleted\. In non\-verbose mode \(the default\), information is returned only for those cases in which there was a validation failure\. 

### Example<a name="cloudtrail-log-file-validation-cli-validate-logs-example"></a>

The following example validates log files from the specified start time to the present, using the Amazon S3 bucket configured for the current trail and specifying verbose output\.

```
aws cloudtrail validate-logs --start-time 2015-08-27T00:00:00Z --end-time 2015-08-28T00:00:00Z --trail-arn arn:aws:cloudtrail:us-east-2:111111111111:trail/my-trail-name --verbose
```

### How `validate-logs` Works<a name="cloudtrail-log-file-validation-cli-validate-logs-how-it-works"></a>

 The `validate-logs` command starts by validating the most recent digest file in the specified time range\. First, it verifies that the digest file has been downloaded from the location to which it claims to belong\. In other words, if the CLI downloads digest file `df1` from the S3 location `p1`, validate\-logs will verify that `p1 == df1.digestS3Bucket + '/' + df1.digestS3Object`\.

 If the signature of the digest file is valid, it checks the hash value of each of the logs referenced in the digest file\. The command then goes back in time, validating the previous digest files and their referenced log files in succession\. It continues until the specified value for `start-time` is reached, or until the digest chain ends\. If a digest file is missing or not valid, the time range that cannot be validated is indicated in the output\. 

## Validation Results<a name="cloudtrail-log-file-validation-cli-results"></a>

Validation results begin with a summary header in the following format:

```
Validating log files for trail trail_ARN  between time_stamp and time_stamp
```

Each line of the main output contains the validation results for a single digest or log file in the following format:

```
<Digest file | Log file> <S3 path> <Validation Message>
```

The following table describes the possible validation messages for log and digest files\.


****  

| File Type | Validation Message | Description | 
| --- | --- | --- | 
| Digest file | valid | The digest file signature is valid\. The log files it references can be checked\. This message is included only in verbose mode\. | 
| Digest file | INVALID: has been moved from its original location | The S3 bucket or S3 object from which the digest file was retrieved do not match the S3 bucket or S3 object locations that are recorded in the digest file itself\. | 
| Digest file | INVALID: invalid format | The format of the digest file is invalid\. The log files corresponding to the time range that the digest file represents cannot be validated\. | 
| Digest file | INVALID: not found | The digest file was not found\. The log files corresponding to the time range that the digest file represents cannot be validated\. | 
| Digest file | INVALID: public key not found for fingerprint fingerprint | The public key corresponding to the fingerprint recorded in the digest file was not found\. The digest file cannot be validated\. | 
| Digest file | INVALID: signature verification failed | The digest file signature is not valid\. Because the digest file is not valid, the log files it references cannot be validated, and no assertions can be made about the API activity in them\. | 
| Digest file | INVALID: Unable to load PKCS \#1 key with fingerprint fingerprint | Because the DER encoded public key in PKCS \#1 format having the specified fingerprint could not be loaded, the digest file cannot be validated\. | 
| Log file | valid | The log file has been validated and has not been modified since the time of delivery\. This message is included only in verbose mode\. | 
| Log file | INVALID: hash value doesn't match | The hash for the log file does not match\. The log file has been modified after delivery by CloudTrail\. | 
| Log file | INVALID: invalid format | The format of the log file is invalid\. The log file cannot be validated\. | 
| Log file | INVALID: not found | The log file was not found and cannot be validated\. | 

The output includes summary information about the results returned\.

## Example Outputs<a name="cloudtrail-log-file-validation-cli-results-examples"></a>

### Verbose<a name="cloudtrail-log-file-validation-cli-results-verbose"></a>

The following example `validate-logs` command uses the `--verbose` flag and produces the sample output that follows\. `[...]` indicates the sample output has been abbreviated\.

```
aws cloudtrail validate-logs --trail-arn arn:aws:cloudtrail:us-east-2:111111111111:trail/example-trail-name --start-time 2015-08-31T22:00:00Z --end-time 2015-09-01T19:17:29Z --verbose
```

```
Validating log files for trail arn:aws:cloudtrail:us-east-2:111111111111:trail/example-trail-name between 2015-08-31T22:00:00Z and 2015-09-01T19:17:29Z
                                       
Digest file    s3://example-bucket/AWSLogs/111111111111/CloudTrail-Digest/us-east-2/2015/09/01/111111111111_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T201728Z.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1925Z_WZZw1RymnjCRjxXc.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1915Z_POuvV87nu6pfAV2W.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1930Z_l2QgXhAKVm1QXiIA.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1920Z_eQJteBBrfpBCqOqw.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1950Z_9g5A6qlR2B5KaRdq.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1920Z_i4DNCC12BuXd6Ru7.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1915Z_Sg5caf2RH6Jdx0EJ.json.gz	valid
Digest file    s3://example-bucket/AWSLogs/111111111111/CloudTrail-Digest/us-east-2/2015/09/01/111111111111_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T191728Z.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/09/01/111111111111_CloudTrail_us-east-2_20150901T1910Z_YYSFiuFQk4nrtnEW.json.gz	valid
[...]
Log file       s3://example-bucket/AWSLogs/144218288521/CloudTrail/us-east-2/2015/09/01/144218288521_CloudTrail_us-east-2_20150901T1055Z_0Sfy6m9f6iBzmoPF.json.gz	valid
Log file       s3://example-bucket/AWSLogs/144218288521/CloudTrail/us-east-2/2015/09/01/144218288521_CloudTrail_us-east-2_20150901T1040Z_lLa3QzVLpOed7igR.json.gz	valid

Digest file    s3://example-bucket/AWSLogs/144218288521/CloudTrail-Digest/us-east-2/2015/09/01/144218288521_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T101728Z.json.gz	INVALID: signature verification failed

Digest file    s3://example-bucket/AWSLogs/144218288521/CloudTrail-Digest/us-east-2/2015/09/01/144218288521_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T091728Z.json.gz	valid
Log file       s3://example-bucket/AWSLogs/144218288521/CloudTrail/us-east-2/2015/09/01/144218288521_CloudTrail_us-east-2_20150901T0830Z_eaFvO3dwHo4NCqqc.json.gz	valid
Digest file    s3://example-bucket/AWSLogs/144218288521/CloudTrail-Digest/us-east-2/2015/09/01/144218288521_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T081728Z.json.gz	valid
Digest file    s3://example-bucket/AWSLogs/144218288521/CloudTrail-Digest/us-east-2/2015/09/01/144218288521_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T071728Z.json.gz	valid
[...]
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/08/31/111111111111_CloudTrail_us-east-2_20150831T2245Z_mbJkEO5kNcDnVhGh.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/08/31/111111111111_CloudTrail_us-east-2_20150831T2225Z_IQ6kXy8sKU03RSPr.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/08/31/111111111111_CloudTrail_us-east-2_20150831T2230Z_eRPVRTxHQ5498ROA.json.gz	valid
Log file       s3://example-bucket/AWSLogs/111111111111/CloudTrail/us-east-2/2015/08/31/111111111111_CloudTrail_us-east-2_20150831T2255Z_IlWawYZGvTWB5vYN.json.gz	valid
Digest file    s3://example-bucket/AWSLogs/111111111111/CloudTrail-Digest/us-east-2/2015/08/31/111111111111_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150831T221728Z.json.gz	valid

Results requested for 2015-08-31T22:00:00Z to 2015-09-01T19:17:29Z
Results found for 2015-08-31T22:17:28Z to 2015-09-01T20:17:28Z:

22/23 digest files valid, 1/23 digest files INVALID
63/63 log files valid
```

### Non\-verbose<a name="cloudtrail-log-file-validation-cli-results-non-verbose"></a>

The following example `validate-logs` command does not use the `--verbose` flag\. In the sample output that follows, one error was found\. Only the header, error, and summary information are returned\.

```
aws cloudtrail validate-logs --trail-arn arn:aws:cloudtrail:us-east-2:111111111111:trail/example-trail-name --start-time 2015-08-31T22:00:00Z --end-time 2015-09-01T19:17:29Z
```

```
Validating log files for trail arn:aws:cloudtrail:us-east-2:111111111111:trail/example-trail-name between 2015-08-31T22:00:00Z and 2015-09-01T19:17:29Z

Digest file	s3://example-bucket/AWSLogs/144218288521/CloudTrail-Digest/us-east-2/2015/09/01/144218288521_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150901T101728Z.json.gz	INVALID: signature verification failed

Results requested for 2015-08-31T22:00:00Z to 2015-09-01T19:17:29Z
Results found for 2015-08-31T22:17:28Z to 2015-09-01T20:17:28Z:

22/23 digest files valid, 1/23 digest files INVALID
63/63 log files valid
```

## Checking Whether a Particular File was Delivered by CloudTrail<a name="cloudtrail-log-file-validation-cli-validate-logs-check-file"></a>

To check if a particular file in your bucket was delivered by CloudTrail, run `validate-logs` in verbose mode for the time period that includes the file\. If the file appears in the output of `validate-logs`, then the file was delivered by CloudTrail\.
