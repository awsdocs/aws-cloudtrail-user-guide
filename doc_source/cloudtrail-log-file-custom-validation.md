# Custom Implementations of CloudTrail Log File Integrity Validation<a name="cloudtrail-log-file-custom-validation"></a>

Because CloudTrail uses industry standard, openly available cryptographic algorithms and hash functions, you can create your own tools to validate the integrity of CloudTrail log files\. When log file integrity validation is enabled, CloudTrail delivers digest files to your Amazon S3 bucket\. You can use these files to implement your own validation solution\. For more information about digest files, see [CloudTrail Digest File Structure](cloudtrail-log-file-validation-digest-file-structure.md)\. 

This topic describes how digest files are signed, and then details the steps that you will need to take to implement a solution that validates the digest files and the log files that they reference\. 

## Understanding How CloudTrail Digest Files are Signed<a name="cloudtrail-log-file-custom-validation-how-cloudtrail-digest-files-are-signed"></a>

 CloudTrail digest files are signed with RSA digital signatures\. For each digest file, CloudTrail does the following: 

1. Creates a string for data signing based on designated digest file fields \(described in the next section\)\. 

1. Gets a private key unique to the region\.

1. Passes the SHA\-256 hash of the string and the private key to the RSA signing algorithm, which produces a digital signature\.

1. Encodes the byte code of the signature into hexadecimal format\.

1. Puts the digital signature into the `x-amz-meta-signature` metadata property of the Amazon S3 digest file object\.

### Contents of the Data Signing String<a name="cloudtrail-log-file-custom-validation-data-signing-string-summary"></a>

 The following CloudTrail objects are included in the string for data signing: 

+ The ending timestamp of the digest file in UTC extended format \(for example, `2015-05-08T07:19:37Z`\)

+ The current digest file S3 path

+ The hexadecimal\-encoded SHA\-256 hash of the current digest file

+ The hexadecimal\-encoded signature of the previous digest file

The format for calculating this string and an example string are provided later in this document\.

## Custom Validation Implementation Steps<a name="cloudtrail-log-file-custom-validation-steps"></a>

When implementing a custom validation solution, you will need to validate the digest file first, and then the log files that it references\. 

### Validate the Digest File<a name="cloudtrail-log-file-custom-validation-steps-digest"></a>

 To validate a digest file, you need its signature, the public key whose private key was used to signed it, and a data signing string that you compute\. 

1. Get the digest file\.

1.  Verify that the digest file has been retrieved from its original location\. 

1. Get the hexadecimal\-encoded signature of the digest file\.

1. Get the hexadecimal\-encoded fingerprint of the public key whose private key was used to sign the digest file\.

1. Retrieve the public keys for the time range corresponding to the digest file\.

1. From among the public keys retrieved, choose the public key whose fingerprint matches the fingerprint in the digest file\.

1. Using the digest file hash and other digest file fields, recreate the data signing string used to verify the digest file signature\.

1. Validate the signature by passing in the SHA\-256 hash of the string, the public key, and the signature as parameters to the RSA signature verification algorithm\. If the result is true, the digest file is valid\. 

### Validate the Log Files<a name="cloudtrail-log-file-custom-validation-steps-logs"></a>

If the digest file is valid, validate each of the log files that the digest file references\.

1. To validate the integrity of a log file, compute its SHA\-256 hash value on its uncompressed content and compare the results with the hash for the log file recorded in hexadecimal in the digest\. If the hashes match, the log file is valid\.

1. By using the information about the previous digest file that is included in the current digest file, validate the previous digest files and their corresponding log files in succession\.

The following sections describe these steps in detail\.

### A\. Get the Digest File<a name="cloudtrail-log-file-custom-validation-steps-get-the-digest-file"></a>

The first steps are to get the most recent digest file, verify that you have retrieved it from its original location, verify its digital signature, and get the fingerprint of the public key\.

1. Using [http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) or the [AmazonS3Client class](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3Client.html) \(for example\), get the most recent digest file from your Amazon S3 bucket for the time range that you want to validate\. 

1.  Check that the S3 bucket and S3 object used to retrieve the file match the S3 bucket S3 object locations that are recorded in the digest file itself\. 

1. Next, get the digital signature of the digest file from the `x-amz-meta-signature` metadata property of the digest file object in Amazon S3\.

1. In the digest file, get the fingerprint of the public key whose private key was used to sign the digest file from the `digestPublicKeyFingerprint` field\. 

### B\. Retrieve the Public Key for Validating the Digest File<a name="cloudtrail-log-file-custom-validation-steps-retrieve-public-key"></a>

To get the public key to validate the digest file, you can use either the AWS CLI or the CloudTrail API\. In both cases, you specify a time range \(that is, a start time and end time\) for the digest files that you want to validate\. One or more public keys may be returned for the time range that you specify\. The returned keys may have validity time ranges that overlap\.

**Note**  
Because CloudTrail uses different private/public key pairs per region, each digest file is signed with a private key unique to its region\. Therefore, when you validate a digest file from a particular region, you must retrieve its public key from the same region\.

#### Use the AWS CLI to Retrieve Public Keys<a name="cloudtrail-log-file-custom-validation-steps-retrieve-public-key-cli"></a>

To retrieve public keys for digest files by using the AWS CLI, use the `cloudtrail list-public-keys` command\. The command has the following format: 

 `aws cloudtrail list-public-keys [--start-time <start-time>] [--end-time <end-time>]` 

The start\-time and end\-time parameters are UTC timestamps and are optional\. If not specified, the current time is used, and the currently active public key or keys are returned\.

 **Sample Response** 

The response will be a list of JSON objects representing the key \(or keys\) returned: 

```
{
    "publicKeyList": [
        {
            "ValidityStartTime": "1436317441.0",
            "ValidityEndTime": "1438909441.0",
            "Value": "MIIBCgKCAQEAn11L2YZ9h7onug2ILi1MWyHiMRsTQjfWE+pHVRLk1QjfWhirG+lpOa8NrwQ/r7Ah5bNL6HepznOU9XTDSfmmnP97mqyc7z/upfZdS/AHhYcGaz7n6Wc/RRBU6VmiPCrAUojuSk6/GjvA8iOPFsYDuBtviXarvuLPlrT9kAd4Lb+rFfR5peEgBEkhlzc5HuWO7S0y+KunqxX6jQBnXGMtxmPBPP0FylgWGNdFtks/4YSKcgqwH0YDcawP9GGGDAeCIqPWIXDLG1jOjRRzWfCmD0iJUkz8vTsn4hq/5ZxRFE7UBAUiVcGbdnDdvVfhF9C3dQiDq3k7adQIziLT0cShgQIDAQAB",
            "Fingerprint": "8eba5db5bea9b640d1c96a77256fe7f2"
        },
        {
            "ValidityStartTime": "1434589460.0",
            "ValidityEndTime": "1437181460.0",
            "Value": "MIIBCgKCAQEApfYL2FiZhpN74LNWVUzhR+VheYhwhYm8w0n5Gf6i95ylW5kBAWKVEmnAQG7BvS5g9SMqFDQx52fW7NWV44IvfJ2xGXT+wT+DgR6ZQ+6yxskQNqV5YcXj4Aa5Zz4jJfsYjDuO2MDTZNIzNvBNzaBJ+r2WIWAJ/Xq54kyF63B6WE38vKuDE7nSd1FqQuEoNBFLPInvgggYe2Ym1Refe2z71wNcJ2kY+q0h1BSHrSM8RWuJIw7MXwF9iQncg9jYzUlNJomozQzAG5wSRfbplcCYNY40xvGd/aAmO0m+Y+XFMrKwtLCwseHPvj843qVno6x4BJN9bpWnoPo9sdsbGoiK3QIDAQAB",
            "Fingerprint": "8933b39ddc64d26d8e14ffbf6566fee4"
        },
        {
            "ValidityStartTime": "1434589370.0",
            "ValidityEndTime": "1437181370.0",
            "Value": "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqlzPJbvZJ42UdcmLfPUqXYNfOs6I8lCfao/tOs8CmzPOEdtLWugB9xoIUz78qVHdKIqxbaG4jWHfJBiOSSFBM0lt8cdVo4TnRa7oG9io5pysS6DJhBBAeXsicufsiFJR+wrUNh8RSLxL4k6G1+BhLX20tJkZ/erT97tDGBujAelqseGg3vPZbTx9SMfOLN65PdLFudLP7Gat0Z9p5jw/rjpclKfo9Bfc3heeBxWGKwBBOKnFAaN9V57pOaosCvPKmHd9bg7jsQkI9Xp22IzGLsTFJZYVA3KiTAElDMu80iFXPHEq9hKNbt9e4URFam+1utKVEiLkR2disdCmPTK0VQIDAQAB",
            "Fingerprint": "31e8b5433410dfb61a9dc45cc65b22ff"
        }
    ]
}
```

#### Use the CloudTrail API to Retrieve Public Keys<a name="cloudtrail-log-file-custom-validation-steps-retrieve-public-key-api"></a>

To retrieve public keys for digest files by using the CloudTrail API, pass in start time and end time values to the `ListPublicKeys` API\. The `ListPublicKeys` API returns the public keys whose private keys were used to sign digest files within the specified time range\. For each public key, the API also returns the corresponding fingerprint\.

##### `ListPublicKeys`<a name="cloudtrail-log-file-custom-validation-steps-list-public-keys"></a>

This section describes the request parameters and response elements for the `ListPublicKeys` API\.

**Note**  
 The encoding for the binary fields for `ListPublicKeys` is subject to change\. 

 **Request Parameters** 


****  

| Name | Description | 
| --- | --- | 
|  StartTime  |  Optionally specifies, in UTC, the start of the time range to look up public keys for CloudTrail digest files\. If StartTime is not specified, the current time is used, and the current public key is returned\.  Type: DateTime   | 
|  EndTime  |  Optionally specifies, in UTC, the end of the time range to look up public keys for CloudTrail digest files\. If EndTime is not specified, the current time is used\.  Type: DateTime   | 

 **Response Elements** 

`PublicKeyList`, an array of `PublicKey` objects that contains: 


****  

|  |  | 
| --- |--- |
|  Name  |  Description  | 
|  Value  |  The DER encoded public key value in PKCS \#1 format\.  Type: Blob   | 
|  ValidityStartTime  |  The starting time of validity of the public key\. Type: DateTime   | 
|  ValidityEndTime  |  The ending time of validity of the public key\. Type: DateTime   | 
|  Fingerprint  |  The fingerprint of the public key\. The fingerprint can be used to identify the public key that you must use to validate the digest file\. Type: String   | 

### C\. Choose the Public Key to Use for Validation<a name="cloudtrail-log-file-custom-validation-steps-choose-public-key"></a>

 From among the public keys retrieved by `list-public-keys` or `ListPublicKeys`, choose the public key returned whose fingerprint matches the fingerprint recorded in the `digestPublicKeyFingerprint` field of the digest file\. This is the public key that you will use to validate the digest file\. 

### D\. Recreate the Data Signing String<a name="cloudtrail-log-file-custom-validation-steps-recreate-data-signing-string"></a>

Now that you have the signature of the digest file and associated public key, you need to calculate the data signing string\. After you have calculated the data signing string, you will have the inputs needed to verify the signature\.

 The data signing string has the following format: 

```
Data_To_Sign_String = 
  Digest_End_Timestamp_in_UTC_Extended_format + '\n' +
  Current_Digest_File_S3_Path + '\n' +
  Hex(Sha256(current-digest-file-content)) + '\n' +
  Previous_digest_signature_in_hex
```

An example `Data_To_Sign_String` follows\.

```
2015-08-12T04:01:31Z
S3-bucket-name/AWSLogs/111122223333/CloudTrail-Digest/us-east-2/2015/08/12/111122223333_us-east-2_CloudTrail-Digest_us-east-2_20150812T040131Z.json.gz
4ff08d7c6ecd6eb313257e839645d20363ee3784a2328a7d76b99b53cc9bcacd
6e8540b83c3ac86a0312d971a225361d28ed0af20d70c211a2d405e32abf529a8145c2966e3bb47362383a52441545ed091fb81
d4c7c09dd152b84e79099ce7a9ec35d2b264eb92eb6e090f1e5ec5d40ec8a0729c02ff57f9e30d5343a8591638f8b794972ce15bb3063a01972
98b0aee2c1c8af74ec620261529265e83a9834ebef6054979d3e9a6767dfa6fdb4ae153436c567d6ae208f988047ccfc8e5e41f7d0121e54ed66b1b904f80fb2ce304458a2a6b91685b699434b946c52589e9438f8ebe5a0d80522b2f043b3710b87d2cda43e5c1e0db921d8d540b9ad5f6d4$31b1f4a8ef2d758424329583897339493a082bb36e782143ee5464b4e3eb4ef6
```

After you recreate this string, you can validate the digest file\.

### E\. Validate the Digest File<a name="cloudtrail-log-file-custom-validation-steps-validate-digest-file"></a>

 Pass the SHA\-256 hash of the recreated data signing string, digital signature, and public key to the RSA signature verification algorithm\. If the output is true, the signature of the digest file is verified and the digest file is valid\. 

### F\. Validate the Log Files<a name="cloudtrail-log-file-custom-validation-steps-validate-log-files"></a>

 After you have validated the digest file, you can validate the log files it references\. The digest file contains the SHA\-256 hashes of the log files\. If one of the log files was modified after CloudTrail delivered it, the SHA\-256 hashes will change, and the signature of digest file will not match\. 

The following shows how validate the log files:

1. Do an `S3 Get` of the log file using the S3 location information in the digest file's `logFiles.s3Bucket` and `logFiles.s3Object` fields\.

1. If the `S3 Get` operation is successful, iterate through the log files listed in the digest file's logFiles array using the following steps:

   1. Retrieve the original hash of the file from the `logFiles.hashValue` field of the corresponding log in the digest file\.

   1. Hash the uncompressed contents of the log file with the hashing algorithm specified in `logFiles.hashAlgorithm`\.

   1. Compare the hash value that you generated with the one for the log in the digest file\. If the hashes match, the log file is valid\.

### G\. Validate Additional Digest and Log Files<a name="cloudtrail-log-file-custom-validation-steps-validate-additional-files"></a>

In each digest file, the following fields provide the location and signature of the previous digest file:

+  `previousDigestS3Bucket` 

+  `previousDigestS3Object` 

+  `previousDigestSignature` 

Use this information to visit previous digest files sequentially, validating the signature of each and the log files that they reference by using the steps in the previous sections\. The only difference is that for previous digest files, you do not need to retrieve the digital signature from the digest file object's Amazon S3 metadata properties\. The signature for the previous digest file is provided for you in the `previousDigestSignature` field\. 

You can go back until the starting digest file is reached, or until the chain of digest files is broken, whichever comes first\. 

## Validating Digest and Log Files Offline<a name="cloudtrail-log-file-custom-validation-offline"></a>

When validating digest and log files offline, you can generally follow the procedures described in the previous sections\. However, you must take into account the following areas:

### Handling the Most Recent Digest File<a name="cloudtrail-log-file-custom-validation-offline-most-recent-digest"></a>

The digital signature of the most recent \(that is, "current"\) digest file is in the Amazon S3 metadata properties of the digest file object\. In an offline scenario, the digital signature for the current digest file will not be available\.

Two possible ways of handling this are:

+ Since the digital signature for the previous digest file is in the current digest file, start validating from the next\-to\-last digest file\. With this method, the most recent digest file cannot be validated\.

+ As a preliminary step, obtain the signature for the current digest file from the digest file object's metadata properties \(for example, by calling the Amazon S3 [getObjectMetadata](http://docs.aws.amazon.com/AWSJavaSDK/latest/javadoc/com/amazonaws/services/s3/AmazonS3.html#getObjectMetadata(com.amazonaws.services.s3.model.GetObjectMetadataRequest)) API\) and then store it securely offline\. This would allow the current digest file to be validated in addition to the previous files in the chain\.

### Path Resolution<a name="cloudtrail-log-file-custom-validation-offline-path-resolution"></a>

Fields in the downloaded digest files like `s3Object` and `previousDigestS3Object` will still be pointing to Amazon S3 online locations for log files and digest files\. An offline solution must find a way to reroute these to the current path of the downloaded log and digest files\.

### Public Keys<a name="cloudtrail-log-file-custom-validation-offline-public-keys"></a>

In order to validate offline, all of the public keys that you need for validating log files in a given time range must first be obtained online \(by calling `ListPublicKeys`, for example\) and then stored securely offline\. This step must be repeated whenever you want to validate additional files outside the initial time range that you specified\.

## Sample Validation Snippet<a name="cloudtrail-log-file-custom-validation-sample-code"></a>

 The following sample snippet provides skeleton code for validating CloudTrail digest and log files\. The skeleton code is online/offline agnostic; that is, it is up to you to decide whether to implement it with or without online connectivity to AWS\. The suggested implementation uses the [Java Cryptography Extension \(JCE\)](https://en.wikipedia.org/wiki/Java_Cryptography_Extension) and [Bouncy Castle](http://www.bouncycastle.org/) as a security provider\. 

The sample snippet shows:

+ How to create the data signing string used to validate the digest file signature\. 

+ How to verify the digest file signature\.

+ How to verify the log file hashes\.

+ A code structure for validating a chain of digest files\.

```
import java.util.Arrays;
import java.security.MessageDigest;
import java.security.KeyFactory;
import java.security.PublicKey;
import java.security.Security;
import java.security.Signature;
import java.security.spec.X509EncodedKeySpec;
import org.json.JSONObject;
import org.bouncycastle.jce.provider.BouncyCastleProvider;
import org.apache.commons.codec.binary.Hex;
 
public class DigestFileValidator {
 
    public void validateDigestFile(String digestS3Bucket, String digestS3Object, String digestSignature) {
 
        // Using the Bouncy Castle provider as a JCE security provider - http://www.bouncycastle.org/
        Security.addProvider(new BouncyCastleProvider());
 
        // Load the digest file from S3 (using Amazon S3 Client) or from your local copy
        JSONObject digestFile = loadDigestFileInMemory(digestS3Bucket, digestS3Object);
 
        // Check that the digest file has been retrieved from its original location
        if (!digestFile.getString("digestS3Bucket").equals(digestS3Bucket) ||
                !digestFile.getString("digestS3Object").equals(digestS3Object)) {
            System.err.println("Digest file has been moved from its original location.");
        } else {
            // Compute digest file hash
            MessageDigest messageDigest = MessageDigest.getInstance("SHA-256");
            messageDigest.update(convertToByteArray(digestFile));
            byte[] digestFileHash = messageDigest.digest();
            messageDigest.reset();
 
            // Compute the data to sign
            String dataToSign = String.format("%s%n%s/%s%n%s%n%s",
                                digestFile.getString("digestEndTime"),
                                digestFile.getString("digestS3Bucket"), digestFile.getString("digestS3Object"), // Constructing the S3 path of the digest file as part of the data to sign
                                Hex.encodeHexString(digestFileHash),
                                digestFile.getString("previousDigestSignature"));
 
            byte[] signatureContent = Hex.decodeHex(digestSignature);
 
            /*
                NOTE: 
                To find the right public key to verify the signature, call CloudTrail ListPublicKey API to get a list 
                of public keys, then match by the publicKeyFingerprint in the digest file. Also, the public key bytes 
                returned from ListPublicKey API are DER encoded in PKCS#1 format:
 
                PublicKeyInfo ::= SEQUENCE {
                    algorithm       AlgorithmIdentifier,
                    PublicKey       BIT STRING
                }
 
                AlgorithmIdentifier ::= SEQUENCE {
                    algorithm       OBJECT IDENTIFIER,
                    parameters      ANY DEFINED BY algorithm OPTIONAL
                }                
            */
            pkcs1PublicKeyBytes = getPublicKey(digestFile.getString("digestPublicKeyFingerprint")));
 
            // Transform the PKCS#1 formatted public key to x.509 format.
            RSAPublicKey rsaPublicKey = RSAPublicKey.getInstance(pkcs1PublicKeyBytes);
            AlgorithmIdentifier rsaEncryption = new AlgorithmIdentifier(PKCSObjectIdentifiers.rsaEncryption, null);
            SubjectPublicKeyInfo publicKeyInfo = new SubjectPublicKeyInfo(rsaEncryption, rsaPublicKey);
 
            // Create the PublicKey object needed for the signature validation
            PublicKey publicKey = KeyFactory.getInstance("RSA", "BC").generatePublic(new X509EncodedKeySpec(publicKeyInfo.getEncoded()));
 
            // Verify signature
            Signature signature = Signature.getInstance("SHA256withRSA", "BC");
            signature.initVerify(publicKey);
            signature.update(dataToSign.getBytes("UTF-8"));
 
            if (signature.verify(signatureContent)) {
                System.out.println("Digest file signature is valid, validating log files…");
                for (int i = 0; i < digestFile.getJSONArray("logFiles").length(); i++) {
 
                    JSONObject logFileMetadata = digestFile.getJSONArray("logFiles").getJSONObject(i);
 
                    // Compute log file hash
                    byte[] logFileContent = loadUncompressedLogFileInMemory(
                                                logFileMetadata.getString("s3Bucket"),
                                                logFileMetadata.getString("s3Object")
                                            );
                    messageDigest.update(logFileContent);
                     byte[] logFileHash = messageDigest.digest();
                    messageDigest.reset();
 
                    // Retrieve expected hash for the log file being processed
                    byte[] expectedHash = Hex.decodeHex(logFileMetadata.getString("hashValue"));
 
                    boolean signaturesMatch = Arrays.equals(expectedHash, logFileHash);
                    if (!signaturesMatch) {
                        System.err.println(String.format("Log file: %s/%s hash doesn't match.\tExpected: %s Actual: %s",
                               logFileMetadata.getString("s3Bucket"), logFileMetadata.getString("s3Object"),
                               Hex.encodeHexString(expectedHash), Hex.encodeHexString(logFileHash)));
                    } else {
                        System.out.println(String.format("Log file: %s/%s hash match",
                               logFileMetadata.getString("s3Bucket"), logFileMetadata.getString("s3Object")));
                    }
                }
 
            } else {
                System.err.println("Digest signature failed validation.");
            }
 
            System.out.println("Digest file validation completed.");
 
            if (chainValidationIsEnabled()) {
                // This enables the digests' chain validation
                validateDigestFile(
                        digestFile.getString("previousDigestS3Bucket"),
                        digestFile.getString("previousDigestS3Object"),
                        digestFile.getString("previousDigestSignature"));
            }
        }
    }
}
```