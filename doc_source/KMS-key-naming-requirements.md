# AWS KMS alias naming requirements<a name="KMS-key-naming-requirements"></a>

When you create an AWS KMS key, you can choose an alias to identify it\. For example, you might choose the alias "KMS\-CloudTrail\-us\-west\-2" to encrypt the logs for a specific trail\.

The alias must meet the following requirements:
+ Between 1 and 256 characters, inclusive
+ Contain alphanumeric characters \(A\-Z, a\-z, 0\-9\), hyphens \(\-\), forward slashes \(/\), and underscores \(\_\)
+ Cannot begin with **aws**

For more information, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\.