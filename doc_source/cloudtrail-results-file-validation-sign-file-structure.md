# CloudTrail sign file structure<a name="cloudtrail-results-file-validation-sign-file-structure"></a>

The sign file contains the name of each query result file that was delivered to your Amazon S3 bucket when you saved the query results, the hash value for each query result file, and the digital signature of the file\. The digital signature and hash values are used for validating the integrity of the query result files and of the sign file itself\. 

## Sign file location<a name="cloudtrail-results-file-validation-sign-file-location"></a>

The sign file is delivered to an Amazon S3 bucket location that follows this syntax\.

```
s3://s3-bucket-name/optional-prefix/AWSLogs/aws-account-ID/CloudTrail-Lake/Query/year/month/date/query-ID/result_sign.json
```

## Sample sign file contents<a name="cloudtrail-results-file-validation-sign-file-contents"></a>

The following example sign file contains information for CloudTrail Lake query results\.

```
{
  "version": "1.0",
  "region": "us-east-1",
  "files": [
    {
      "fileHashValue" : "de85a48b8a363033c891abd723181243620a3af3b6505f0a44db77e147e9c188",
      "fileName" : "result_1.csv.gz"
    }
  ],
  "hashAlgorithm" : "SHA-256",
  "signatureAlgorithm" : "SHA256withRSA",
  "queryCompleteTime": "2022-05-10T22:06:30Z",
  "hashSignature" : "7664652aaf1d5a17a12ba50abe6aca77c0ec76264bdf7dce71ac6d1c7781117c2a412e5820bccf473b1361306dff648feae20083ad3a27c6118172a81635829bdc7f7b795ebfabeb5259423b2fb2daa7d1d02f55791efa403dac553171e7ce5f9307d13e92eeec505da41685b4102c71ec5f1089168dacde702c8d39fed2f25e9216be5c49769b9db51037cb70a84b5712e1dffb005a74580c7fdcbb89a16b9b7674e327de4f5414701a772773a4c98eb008cca34228e294169901c735221e34cc643ead34628aabf1ba2c32e0cdf28ef403e8fe3772499ac61e21b70802dfddded9bea0ddfc3a021bf2a0b209f312ccee5a43f2b06aa35cac34638f7611e5d7",
  "publicKeyFingerprint" : "67b9fa73676d86966b449dd677850753"
}
```

## Sign file field descriptions<a name="cloudtrail-results-file-validation-sign-file-descriptions"></a>

The following are descriptions for each field in the sign file: 

`version`  
The version of the sign file\. 

`region`  
The region for the AWS account used for saving the query results\. 

`files.fileHashValue`  
The hexadecimal encoded hash value of the compressed query result file content\.

`files.fileName`  
The name of the query result file\. 

`hashAlgorithm`  
The hash algorithm used to hash the query result file\. 

`signatureAlgorithm`  
The algorithm used to sign the file\. 

`queryCompleteTime`  
Indicates when CloudTrail delivered the query results to the S3 bucket\. You can use this value to find the public key\.

`hashSignature`  
The hash signature for the file\.

`publicKeyFingerprint`  
The hexadecimal encoded fingerprint of the public key used to sign the file\.