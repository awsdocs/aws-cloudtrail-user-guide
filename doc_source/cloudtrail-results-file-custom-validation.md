# Custom implementations of CloudTrail query result file integrity validation<a name="cloudtrail-results-file-custom-validation"></a>

Because CloudTrail uses industry standard, openly available cryptographic algorithms and hash functions, you can create your own tools to validate the integrity of the CloudTrail query result files\. When you save query results to an Amazon S3 bucket, CloudTrail delivers a sign file to your S3 bucket\. You can implement your own validation solution to validate the signature and query result files\. For more information about the sign file, see [CloudTrail sign file structure](cloudtrail-results-file-validation-sign-file-structure.md)\. 

This topic describes how the sign file is signed, and then details the steps that you will need to take to implement a solution that validates the sign file and the query result files that the sign file references\. 

## Understanding how CloudTrail sign files are signed<a name="cloudtrail-results-file-custom-validation-how-cloudtrail-sign-files-are-signed"></a>

CloudTrail sign files are signed with RSA digital signatures\. For each sign file, CloudTrail does the following: 

1. Creates a hash list containing the hash value for each query result file\.

1. Gets a private key unique to the region\.

1. Passes the SHA\-256 hash of the string and the private key to the RSA signing algorithm, which produces a digital signature\.

1. Encodes the byte code of the signature into hexadecimal format\.

1. Puts the digital signature into the sign file\.

### Contents of the data signing string<a name="cloudtrail-results-file-custom-validation-data-signing-string-summary"></a>

The data signing string consists of the hash value for each query result file separated by a space\. The sign file lists the `fileHashValue` for each query result file\.

## Custom validation implementation steps<a name="cloudtrail-results-file-custom-validation-steps"></a>

When implementing a custom validation solution, you will need to validate the sign file and the query result files that it references\. 

### Validate the sign file<a name="cloudtrail-results-file-custom-validation-steps-sign"></a>

To validate a sign file, you need its signature, the public key whose private key was used to sign it, and a data signing string that you compute\. 

1. Get the sign file\.

1. Verify that the sign file has been retrieved from its original location\. 

1. Get the hexadecimal\-encoded signature of the sign file\.

1. Get the hexadecimal\-encoded fingerprint of the public key whose private key was used to sign the sign file\.

1. Retrieve the public key for the time range corresponding to `queryCompleteTime` in the sign file\. For the time range, choose a `StartTime` earlier than the `queryCompleteTime` and an `EndTime` later than the `queryCompleteTime`\.

1. From among the public keys retrieved, choose the public key whose fingerprint matches the `publicKeyFingerprint` value in the sign file\.

1. Using a hash list containing the hash value for each query result file separated by a space, recreate the data signing string used to verify the sign file signature\. The sign file lists the `fileHashValue` for each query result file\.

   For example, if your sign file's `files` array contains the following three query result files, your hash list is "aaa bbb ccc"\.

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

1. Validate the signature by passing in the SHA\-256 hash of the string, the public key, and the signature as parameters to the RSA signature verification algorithm\. If the result is true, the sign file is valid\. 

### Validate the query result files<a name="cloudtrail-results-file-custom-validation-steps-logs"></a>

If the sign file is valid, validate the query result files that the sign file references\. To validate the integrity of a query result file, compute its SHA\-256 hash value on its compressed content and compare the results with the `fileHashValue` for the query result file recorded in the sign file\. If the hashes match, the query result file is valid\.

The following sections describe the validation process in detail\.

### A\. Get the sign file<a name="cloudtrail-results-file-custom-validation-steps-get-the-sign-file"></a>

The first steps are to get the sign file and get the fingerprint of the public key\.

1. Get the sign file from your Amazon S3 bucket for the query results that you want to validate\. 

1. Next, get the `hashSignature` value from the sign file\.

1. In the sign file, get the fingerprint of the public key whose private key was used to sign the file from the `publicKeyFingerprint` field\. 

### B\. Retrieve the public key for validating the sign file<a name="cloudtrail-results-file-custom-validation-steps-retrieve-public-key"></a>

To get the public key to validate the sign file, you can use either the AWS CLI or the CloudTrail API\. In both cases, you specify a time range \(that is, a start time and end time\) for the sign file that you want to validate\. Use a time range corresponding to the `queryCompleteTime` in the sign file\. One or more public keys may be returned for the time range that you specify\. The returned keys may have validity time ranges that overlap\.

**Note**  
Because CloudTrail uses different private/public key pairs per region, each sign file is signed with a private key unique to its region\. Therefore, when you validate a sign file from a particular region, you must retrieve its public key from the same region\.

#### Use the AWS CLI to retrieve public keys<a name="cloudtrail-results-file-custom-validation-steps-retrieve-public-key-cli"></a>

To retrieve a public key for a sign file by using the AWS CLI, use the `cloudtrail list-public-keys` command\. The command has the following format: 

 `aws cloudtrail list-public-keys [--start-time <start-time>] [--end-time <end-time>]` 

The start\-time and end\-time parameters are UTC timestamps and are optional\. If not specified, the current time is used, and the currently active public key or keys are returned\.

 **Sample Response** 

The response will be a list of JSON objects representing the key \(or keys\) returned: 

#### Use the CloudTrail API to retrieve public keys<a name="cloudtrail-results-file-custom-validation-steps-retrieve-public-key-api"></a>

To retrieve a public key for a sign file by using the CloudTrail API, pass in start time and end time values to the `ListPublicKeys` API\. The `ListPublicKeys` API returns the public keys whose private keys were used to sign the file within the specified time range\. For each public key, the API also returns the corresponding fingerprint\.

##### `ListPublicKeys`<a name="cloudtrail-results-file-custom-validation-steps-list-public-keys"></a>

This section describes the request parameters and response elements for the `ListPublicKeys` API\.

**Note**  
The encoding for the binary fields for `ListPublicKeys` is subject to change\. 

 **Request Parameters** 


****  

| Name | Description | 
| --- | --- | 
|  StartTime  |  Optionally specifies, in UTC, the start of the time range to look up the public key for CloudTrail sign file\. If StartTime is not specified, the current time is used, and the current public key is returned\.  Type: DateTime   | 
|  EndTime  |  Optionally specifies, in UTC, the end of the time range to look up public keys for CloudTrail sign files\. If EndTime is not specified, the current time is used\.  Type: DateTime   | 

 **Response Elements** 

`PublicKeyList`, an array of `PublicKey` objects that contains: 


****  

|  |  | 
| --- |--- |
|  Name  |  Description  | 
|  Value  |  The DER encoded public key value in PKCS \#1 format\.  Type: Blob   | 
|  ValidityStartTime  |  The starting time of validity of the public key\. Type: DateTime   | 
|  ValidityEndTime  |  The ending time of validity of the public key\. Type: DateTime   | 
|  Fingerprint  |  The fingerprint of the public key\. The fingerprint can be used to identify the public key that you must use to validate the sign file\. Type: String   | 

### C\. Choose the public key to use for validation<a name="cloudtrail-results-file-custom-validation-steps-choose-public-key"></a>

From among the public keys retrieved by `list-public-keys` or `ListPublicKeys`, choose the public key whose fingerprint matches the fingerprint recorded in the `publicKeyFingerprint` field of the sign file\. This is the public key that you will use to validate the sign file\. 

### D\. Recreate the data signing string<a name="cloudtrail-results-file-custom-validation-steps-recreate-data-signing-string"></a>

Now that you have the signature of the sign file and the associated public key, you need to calculate the data signing string\. After you have calculated the data signing string, you will have the inputs needed to verify the signature\.

The data signing string consists of the hash value for each query result file separated by a space\. After you recreate this string, you can validate the sign file\.

### E\. Validate the sign file<a name="cloudtrail-results-file-custom-validation-steps-validate-sign-file"></a>

Pass the recreated data signing string, digital signature, and public key to the RSA signature verification algorithm\. If the output is true, the signature of the sign file is verified and the sign file is valid\. 

### F\. Validate the query result files<a name="cloudtrail-results-file-custom-validation-steps-validate-log-files"></a>

After you have validated the sign file, you can validate the query result files it references\. The sign file contains the SHA\-256 hashes of the query result files\. If one of the query result files was modified after CloudTrail delivered it, the SHA\-256 hashes will change, and the signature of the sign file will not match\. 

Use the following procedure to validate the query result files listed in the sign file's `files` array\.

1. Retrieve the original hash of the file from the `files.fileHashValue` field in the sign file\.

1. Hash the compressed contents of the query result file with the hashing algorithm specified in `hashAlgorithm`\.

1. Compare the hash value that you generated for each query result file with the `files.fileHashValue` in the sign file\. If the hashes match, the query result files are valid\.

## Validating signature and query result files offline<a name="cloudtrail-results-file-custom-validation-offline"></a>

When validating sign and query result files offline, you can generally follow the procedures described in the previous sections\. However, you must take into account the following information about public keys\.

### Public keys<a name="cloudtrail-results-file-custom-validation-offline-public-keys"></a>

In order to validate offline, the public key that you need for validating query result files in a given time range must first be obtained online \(by calling `ListPublicKeys`, for example\) and then stored offline\. This step must be repeated whenever you want to validate additional files outside the initial time range that you specified\.

## Sample validation snippet<a name="cloudtrail-results-file-custom-validation-sample-code"></a>

The following sample snippet provides skeleton code for validating CloudTrail sign and query result files\. The skeleton code is online/offline agnostic; that is, it is up to you to decide whether to implement it with or without online connectivity to AWS\. The suggested implementation uses the [Java Cryptography Extension \(JCE\)](https://en.wikipedia.org/wiki/Java_Cryptography_Extension) and [Bouncy Castle](http://www.bouncycastle.org/) as a security provider\. 

The sample snippet shows:
+ How to create the data signing string used to validate the sign file signature\.
+ How to verify the sign file's signature\.
+ How to calculate the hash value for the query result file and compare it with the `fileHashValue` listed in the sign file to verify the authenticity of the query result file\.

```
import org.apache.commons.codec.binary.Hex;
import org.bouncycastle.asn1.pkcs.PKCSObjectIdentifiers;
import org.bouncycastle.asn1.pkcs.RSAPublicKey;
import org.bouncycastle.asn1.x509.AlgorithmIdentifier;
import org.bouncycastle.asn1.x509.SubjectPublicKeyInfo;
import org.bouncycastle.jce.provider.BouncyCastleProvider;
import org.json.JSONArray;
import org.json.JSONObject;
 
import java.security.KeyFactory;
import java.security.MessageDigest;
import java.security.PublicKey;
import java.security.Security;
import java.security.Signature;
import java.security.spec.X509EncodedKeySpec;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
 
 
public class SignFileValidationSampleCode {
 
    public void validateSignFile(String s3Bucket, String s3PrefixPath) throws Exception {
        MessageDigest messageDigest = MessageDigest.getInstance("SHA-256");
 
        // Load the sign file from S3 (using Amazon S3 Client) or from your local copy
        JSONObject signFile = loadSignFileToMemory(s3Bucket, String.format("%s/%s", s3PrefixPath, "result_sign.json"));
 
        // Using the Bouncy Castle provider as a JCE security provider - http://www.bouncycastle.org/
        Security.addProvider(new BouncyCastleProvider());
 
        List<String> hashList = new ArrayList<>();
 
        JSONArray jsonArray = signFile.getJSONArray("files");
 
        for (int i = 0; i < jsonArray.length(); i++) {
            JSONObject file = jsonArray.getJSONObject(i);
            String fileS3ObjectKey = String.format("%s/%s", s3PrefixPath, file.getString("fileName"));
 
            // Load the export file from S3 (using Amazon S3 Client) or from your local copy
            byte[] exportFileContent = loadCompressedExportFileInMemory(s3Bucket, fileS3ObjectKey);
            messageDigest.update(exportFileContent);
            byte[] exportFileHash = messageDigest.digest();
            messageDigest.reset();
            byte[] expectedHash = Hex.decodeHex(file.getString("fileHashValue"));
 
            boolean signaturesMatch = Arrays.equals(expectedHash, exportFileHash);
            if (!signaturesMatch) {
                System.err.println(String.format("Export file: %s/%s hash doesn't match.\tExpected: %s Actual: %s",
                        s3Bucket, fileS3ObjectKey,
                        Hex.encodeHexString(expectedHash), Hex.encodeHexString(exportFileHash)));
            } else {
                System.out.println(String.format("Export file: %s/%s hash match",
                        s3Bucket, fileS3ObjectKey));
            }
 
            hashList.add(file.getString("fileHashValue"));
        }
        String hashListString = hashList.stream().collect(Collectors.joining(" "));
 
        /*
            NOTE:
            To find the right public key to verify the signature, call CloudTrail ListPublicKey API to get a list
            of public keys, then match by the publicKeyFingerprint in the sign file. Also, the public key bytes
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
        byte[] pkcs1PublicKeyBytes = getPublicKey(signFile.getString("queryCompleteTime"),
                signFile.getString("publicKeyFingerprint"));
        byte[] signatureContent = Hex.decodeHex(signFile.getString("hashSignature"));
 
        // Transform the PKCS#1 formatted public key to x.509 format.
        RSAPublicKey rsaPublicKey = RSAPublicKey.getInstance(pkcs1PublicKeyBytes);
        AlgorithmIdentifier rsaEncryption = new AlgorithmIdentifier(PKCSObjectIdentifiers.rsaEncryption, null);
        SubjectPublicKeyInfo publicKeyInfo = new SubjectPublicKeyInfo(rsaEncryption, rsaPublicKey);
 
        // Create the PublicKey object needed for the signature validation
        PublicKey publicKey = KeyFactory.getInstance("RSA", "BC")
                .generatePublic(new X509EncodedKeySpec(publicKeyInfo.getEncoded()));
 
        // Verify signature
        Signature signature = Signature.getInstance("SHA256withRSA", "BC");
        signature.initVerify(publicKey);
        signature.update(hashListString.getBytes("UTF-8"));
 
        if (signature.verify(signatureContent)) {
            System.out.println("Sign file signature is valid.");
        } else {
            System.err.println("Sign file signature failed validation.");
        }
 
        System.out.println("Sign file validation completed.");
    }
}
```