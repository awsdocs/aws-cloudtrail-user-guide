# Validate query results with the command line<a name="cloudtrail-query-results-validation-cli"></a>

You can validate the integrity of the query result and sign files using the command line\.

## Prerequisites<a name="cloudtrail-query-results-valication-cli-prerequisites"></a>

To validate query results integrity with the command line, the following conditions must be met:
+ You must have online connectivity to AWS\.
+ You must have read access to the Amazon S3 bucket that contains the sign and query result files\. 
+ You must have installed [OpenSSL](https://www.openssl.org/source/)\.

## Validate the query results<a name="cloudtrail-query-results-valication-cli-steps"></a>

Use the following procedure to validate the query results using the command line\.

1. Download the query result and sign files from the S3 bucket using the `aws S3 sync` command\. You must specify the S3 URI where the query result and sign files were delivered, and the S3 bucket region\.

   ```
   aws s3 sync s3://s3-bucket-name/optional-prefix/AWSLogs/aws-account-ID/CloudTrail-Lake/Query/year/month/date/query-ID/ ./ --region region-name
   ```

1. Use the `openssl dgst` command to calculate the sha256 hash value of each query result file, and then compare the value to the hash value in the sign file\. For more information about the fields contained in the sign file, see [CloudTrail sign file structure](cloudtrail-results-file-validation-sign-file-structure.md)\.

   ```
   openssl dgst -sha256 result_number.csv.gz
   ```

1. Use the `printf` command to generate a public key PEM file, which you use to verify the signature\.

   ```
   printf -- "-----BEGIN RSA PUBLIC KEY-----\n%s\n-----END RSA PUBLIC KEY-----" "public_key_content" | fold -w64 >> public_key_pkcs1.pem
   ```

1. Use the `openssl rsa` command to convert the public key PEM file to an OpenSSL loadable format\.

   ```
   openssl rsa -RSAPublicKey_in -in public_key_pkcs1.pem -pubout -out public_key_x509.pem 
   ```

1. Generate a new signature binary file using the `printf` and `xxd` commands\. Replace *hash\_signature* with the `hashSignature` value in the sign file, and *signature* with the name of the new signature file you are creating\.

   ```
   printf "hash_signature" >> signature
   xxd -r -p signature signature.bin
   ```

1. Create a *hash\_list* and output it to a file using the `printf` command\. The *hash\_list* is a string containing the `files.fileHashValue` value for each query result file separated by a space\. The `files.fileHashValue` for each query result file is provided in the sign file\.

   ```
   printf "hash_list" >> hash_list_file
   ```

   For example, if your sign file's `files` array contains the following three query result files, your *hash\_list* value is "aaa bbb ccc"\.

   ```
   “files": [ 
      { 
           "fileHashValue" : “aaa”, 
           "fileName" : "result_1.csv.gz" 
      },
      {       
           "fileHashValue" : “bbb”,       
           "fileName" : "result_2.csv.gz"      
      },
      { 
           "fileHashValue" : “ccc”,       
           "fileName" : "result_3.csv.gz" 
      }
   ],
   ```

1. Validate the signature using the `openssl dgst` command\.

   ```
   openssl dgst -sha256 -verify public_key_x509.pem -signature signature.bin hash_list_file
   ```

   If the signature is correct, the command returns the following output\.

   ```
   Verified OK
   ```

   If the signature is not valid, the command returns the following output\.

   ```
   Verification Failure
   ```