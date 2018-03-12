# CloudTrail Digest File Structure<a name="cloudtrail-log-file-validation-digest-file-structure"></a>

Each digest file contains the names of the log files that were delivered to your Amazon S3 bucket during the last hour, the hash values for those log files, and the digital signature of the previous digest file\. The signature for the current digest file is stored in the metadata properties of the digest file object\. The digital signatures and hashes are used for validating the integrity of the log files and of the digest file itself\. 

## Digest File Location<a name="cloudtrail-log-file-validation-digest-file-location"></a>

 Digest files are delivered to an Amazon S3 bucket location that follows this syntax\.

```
s3://s3-bucket-name/AWSLogs/aws-account-id/CloudTrail-Digest/
    region/digest-end-year/digest-end-month/digest-end-date/
    aws-account-id_CloudTrail-Digest_region_trail-name_region_digest_end_timestamp.json.gz
```

## Sample Digest File Contents<a name="cloudtrail-log-file-validation-digest-file-contents"></a>

The following example digest file contains information for a CloudTrail log\.

```
{
  "awsAccountId": "111122223333",
  "digestStartTime": "2015-08-17T14:01:31Z",
  "digestEndTime": "2015-08-17T15:01:31Z",
  "digestS3Bucket": "S3-bucket-name",
  "digestS3Object": "AWSLogs/111122223333/CloudTrail-Digest/us-east-2/2015/08/17/111122223333_CloudTrail-Digest_us-east-2_your-trail-name_us-east-2_20150817T150131Z.json.gz",
  "digestPublicKeyFingerprint": "31e8b5433410dfb61a9dc45cc65b22ff",
  "digestSignatureAlgorithm": "SHA256withRSA",
  "newestEventTime": "2015-08-17T14:52:27Z",
  "oldestEventTime": "2015-08-17T14:42:27Z",
  "previousDigestS3Bucket": "S3-bucket-name",
  "previousDigestS3Object": "AWSLogs/111122223333/CloudTrail-Digest/us-east-2/2015/08/17/111122223333_CloudTrail-Digest_us-east-2_your-trail-name_us-east-2_20150817T140131Z.json.gz",
  "previousDigestHashValue": "97fb791cf91ffc440d274f8190dbdd9aa09c34432aba82739df18b6d3c13df2d",
  "previousDigestHashAlgorithm": "SHA-256",
  "previousDigestSignature": "50887ccffad4c002b97caa37cc9dc626e3c680207d41d27fa5835458e066e0d3652fc4dfc30937e4d5f4cc7f796e7a258fb50a43ac427f2237f6e505d4efaf373d156e15e3b68dea9f58111d395b62628d6bd367a9024d2183b5c5f6e19466d3a996b92df705bc997b8a0e13430f241d733cf95df4e41bb6c304c3f58363043572ea57a27085639ce187e679c0d81c7519b1184fa77fb7ab0b0e40a32dace6e1eefc3995c5ae182da49b62b26398cebb52a2201a6387b75b89c83e5570bcb9bba6c34a80f2f00a1c6ebe07d1ff149eccd812dc805bb3eeff6657db32a6cb48d2d096404eb76181877bc6ebb8cd0b23f823200155b2fd8848d428e46e8456328a",
  "logFiles": [
    {
      "s3Bucket": "S3-bucket-name",
      "s3Object": "AWSLogs/111122223333/CloudTrail/us-east-2/2015/08/17/111122223333_CloudTrail_us-east-2_20150817T1445Z_9nYN7gp2eWAJHIfT.json.gz",
      "hashValue": "9bb6196fc6b84d6f075a56548feca262bd99ba3c2de41b618e5b6e22c1fc71f6",
      "hashAlgorithm": "SHA-256",
      "newestEventTime": "2015-08-17T14:52:27Z",
      "oldestEventTime": "2015-08-17T14:42:27Z"
    }
  ]
}
```

## Digest File Field Descriptions<a name="cloudtrail-log-file-validation-digest-file-descriptions"></a>

 The following are descriptions for each field in the digest file: 

`awsAccountId`  
The AWS account ID for which the digest file has been delivered\. 

`digestStartTime`  
The starting UTC time range that the digest file covers, taking as a reference the time in which log files have been delivered by CloudTrail\. This means that if the time range is \[Ta, Tb\], the digest will contain all the log files delivered to the customer between Ta and Tb\. 

`digestEndTime`  
The ending UTC time range that the digest file covers, taking as a reference the time in which log files have been delivered by CloudTrail\. This means that if the time range is \[Ta, Tb\], the digest will contain all the log files delivered to the customer between Ta and Tb\. 

`digestS3Bucket`  
The name of the Amazon S3 bucket to which the current digest file has been delivered\. 

`digestS3Object`  
The Amazon S3 object key \(that is, the Amazon S3 bucket location\) of the current digest file\. The first two regions in the string show the region from which the digest file was delivered\. The last region \(after `your-trail-name`\) is the home region of the trail\. The home region is the region in which the trail was created\. In the case of a multi\-region trail, this can be different from the region from which the digest file was delivered\.

`newestEventTime`  
The UTC time of the most recent event among all of the events in the log files in the digest\. 

`oldestEventTime`  
The UTC time of the oldest event among all of the events in the log files in the digest\.   
 If the digest file is delivered late, the value of `oldestEventTime` will be earlier than the value of `digestStartTime`\. 

`previousDigestS3Bucket`  
The Amazon S3 bucket to which the previous digest file was delivered\. 

`previousDigestS3Object`  
The Amazon S3 object key \(that is, the Amazon S3 bucket location\) of the previous digest file\. 

`previousDigestHashValue`  
The hexadecimal encoded hash value of the uncompressed contents of the previous digest file\. 

`previousDigestHashAlgorithm`  
The name of the hash algorithm that was used to hash the previous digest file\. 

`publicKeyFingerprint`  
The hexadecimal encoded fingerprint of the public key that matches the private key used to sign this digest file\. You can retrieve the public keys for the time range corresponding to the digest file by using the AWS CLI or the CloudTrail API\. Of the public keys returned, the one whose fingerprint matches this value can be used for validating the digest file\. For information about retrieving public keys for digest files, see the AWS CLI [http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/list-public-keys.html](http://docs.aws.amazon.com/cli/latest/reference/cloudtrail/list-public-keys.html) command or the CloudTrail [http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_ListPublicKeys.html](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_ListPublicKeys.html) API\.   
 CloudTrail uses different private/public key pairs per region\. Each digest file is signed with a private key unique to its region\. Therefore, when you validate a digest file from a particular region, you must look in the same region for its corresponding public key\. 

`digestSignatureAlgorithm`  
The algorithm used to sign the digest file\. 

`logFiles.s3Bucket`  
The name of the Amazon S3 bucket for the log file\. 

`logFiles.s3Object`  
The Amazon S3 object key of the current log file\. 

`logFiles.newestEventTime`  
The UTC time of the most recent event in the log file\. This time also corresponds to the time stamp of the log file itself\. 

`logFiles.oldestEventTime`  
The UTC time of the oldest event in the log file\. 

`logFiles.hashValue`  
The hexadecimal encoded hash value of the uncompressed log file content\. 

`logFiles.hashAlgorithm`  
The hash algorithm used to hash the log file\. 

## Starting Digest File<a name="cloudtrail-log-file-validation-digest-file-starting"></a>

 When log file integrity validation is started, a starting digest file will be generated\. A starting digest file will also be generated when log file integrity validation is restarted \(by either disabling and then reenabling log file integrity validation, or by stopping logging and then restarting logging with validation enabled\)\. In a starting digest file, the following fields relating to the previous digest file will be null:

+ `previousDigestS3Bucket`

+ `previousDigestS3Object`

+ `previousDigestHashValue`

+ `previousDigestHashAlgorithm`

+ `previousDigestSignature`

## 'Empty' Digest Files<a name="cloudtrail-log-file-validation-digest-file-empty"></a>

 CloudTrail will deliver a digest file even when there has been no API activity in your account during the one hour period that the digest file represents\. This can be useful when you need to assert that no log files were delivered during the hour reported by the digest file\. 

 The following example shows the contents of a digest file that recorded an hour when no API activity occurred\. Note that the `logFiles:[ ]` field at the end of the digest file contents is empty\. 

```
{
  "awsAccountId": "111122223333",
  "digestStartTime": "2015-08-20T17:01:31Z",
  "digestEndTime": "2015-08-20T18:01:31Z",
  "digestS3Bucket": "example-bucket-name",
  "digestS3Object": "AWSLogs/111122223333/CloudTrail-Digest/us-east-2/2015/08/20/111122223333_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150820T180131Z.json.gz",
  "digestPublicKeyFingerprint": "31e8b5433410dfb61a9dc45cc65b22ff",
  "digestSignatureAlgorithm": "SHA256withRSA",
  "newestEventTime": null,
  "oldestEventTime": null,
  "previousDigestS3Bucket": "example-bucket-name",
  "previousDigestS3Object": "AWSLogs/111122223333/CloudTrail-Digest/us-east-2/2015/08/20/111122223333_CloudTrail-Digest_us-east-2_example-trail-name_us-east-2_20150820T170131Z.json.gz",
  "previousDigestHashValue": "ed96c4bac9eaa8fe9716ca0e515da51938be651b1db31d781956416a9d05cdfa",
  "previousDigestHashAlgorithm": "SHA-256",
  "previousDigestSignature": "82705525fb0fe7f919f9434e5b7138cb41793c776c7414f3520c0242902daa8cc8286b29263d2627f2f259471c745b1654af76e2073264b2510fd45236b3aea4d80c0e8e6455223d7bd54ff80af0edf22a5f14fa856626daec919f0591479aa4f213787ba1e1076328dcf8ff624e03a977fa5612dcf58594c590fd8c1c5b48bddf43fc84ecc00b41bedd0ff7f293c3e2de8dcdc78f98b03e17577f5822ba842399d69eb79921c0429773509520e08c8b518702d987dfbb3a4e5d8c5f17673ce1f989dfff82d4becf24e452f20d3bcac94ad50131f93e57f10155536acb54c60efbe9d57228c2b930bc6082b2318e3ccd36834a8e835b8d112dbf32145f445c11",
  "logFiles": []
}
```

## Signature of the Digest File<a name="cloudtrail-log-file-validation-digest-file-signature"></a>

 The signature information for a digest file is located in two object metadata properties of the Amazon S3 digest file object\. Each digest file has the following metadata entries: 

+ `x-amz-meta-signature`

  The hexadecimal encoded value of the digest file signature\. The following is an example signature:

  ```
  3be472336fa2989ef34de1b3c1bf851f59eb030eaff3e2fb6600a082a23f4c6a82966565b994f9de4a5989d053d9d15d20fc5c43e66358652d93326550a4acc5c5f541bb52e9b455897ab723bd7cbabfe963a406a41d600f3658f7a3135e5ed9fcae7b79bb5857d1e5eb78fcce8595ce0ade2f3ad1d9f2d62be7bc4660d83166ce24586489b7da9ee9883eaf0b9efabb5dd3cbba565cc4aab5c9c46c9fa7e9cda310afcc5e8adcd9e48d0597ec5f8174a52c3bebb3e845eeb1d18904fbf4cc14cd117080098e10022ddf55e017a9431446acad8560de0ba1e477af9f8a3048bc6196350adad0cc0cb4ab99b5e7c9944437a3c674a038009220684ced7be07b4f
  28f1cc237f372264a51b611c01da429565def703539f4e71009051769469231bc22232fa260df02740047af532229885ea2b0e95ecd353326b7104941e0cbddb076a391f1fcf2923c19565f4841770a78723451aeb732ff1b6162dc40e601fc6720bc5325987942ebd817783b322f0ac77698523bf742fdea7aa44f4911b3101221b7e1233387f16a52077610498f4a1254211258e37da0fb4cb207aef593b4c1baa13674e85acc52046b3adb889e63331a66abac5de7e42ffdd6952987c31ae871650e130bd2e63bfe145b22bbd39ea192210f6df64d49b888a321e02d3fc4cf126accae30d2857ccd6b2286a7c9feba6c35c44161b24147d645e6ca26844ba
  05d3ffcb5d2dd5dc28f8bb5b7993938e8a5f912a82b448a367eccb2ec0f198ba71e23eb0b97278cf65f3c8d1e652c6de33a22ca8428821ffc95bf8b726ba9f37cfbc20c54dc5bd6159bdea1c4d951b68cb8e0528852c55bb0c5e499ea60560f7c2bb3af7f694407da863a2594f7a2f2838cb09254afbaf8003587746e719a0437f85eeffae534f283f3837eb939a9bccc3c71573500661245891051231b580ac92d9e0e68c6f47ad38975f493e2c40e7f303353c4adc7d563ef1e875977afac2e085f0c824045d998c9543d8a3293ad3c063b7a109d0bfd84b0b1e3f72c4f057e744e6a2cf9cc97727b08584f44bfa47799c5072b60f0b619aea88a17de585e9
  ```

+ `x-amz-meta-signature-algorithm`

  The following shows an example value of the algorithm used to generate the digest signature:

  `SHA256withRSA`

## Digest File Chaining<a name="cloudtrail-log-file-validation-digest-file-chaining"></a>

 The fact that each digest file contains a reference to its previous digest file enables a "chaining" that permits validation tools like the AWS CLI to detect if a digest file has been deleted\. It also allows the digest files in a specified time range to be successively inspected, starting with the most recent first\. 

**Note**  
When you disable log file integrity validation, the chain of digest files is broken after one hour\. CloudTrail will not create digest files for log files that were delivered during a period in which log file integrity validation was disabled\. For example, if you enable log file integrity validation at noon on January 1, disable it at noon on January 2, and re\-enable it at noon on January 10, digest files will not be created for the log files delivered from noon on January 2 to noon on January 10\. The same applies whenever you stop CloudTrail logging or delete a trail\. 

 If logging is stopped or the trail is deleted, CloudTrail will deliver a final digest file\. This digest file can contain information for any remaining log files that cover events up to and including the `StopLogging` event\. 