# Amazon S3 Bucket Naming Requirements<a name="cloudtrail-s3-bucket-naming-requirements"></a>

 The Amazon S3 bucket that you use to store CloudTrail log files must have a name that conforms with naming requirements for non\-US Standard regions\. Amazon S3 defines a bucket name as a series of one or more labels, separated by periods, that adhere to the following rules: 
+  The bucket name can be between 3 and 63 characters long, and can contain only lower\-case characters, numbers, periods, and dashes\. 
+  Each label in the bucket name must start with a lowercase letter or number\. 
+  The bucket name cannot contain underscores, end with a dash, have consecutive periods, or use dashes adjacent to periods\. 
+ The bucket name cannot be formatted as an IP address \(198\.51\.100\.24\)\. 

**Warning**  
Because S3 allows your bucket to be used as a URL that can be accessed publicly, the bucket name that you choose must be globally unique\. If some other account has already created a bucket with the name that you chose, you must use another name\. For more information, see [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) in the *Amazon Simple Storage Service Developer Guide*\. 