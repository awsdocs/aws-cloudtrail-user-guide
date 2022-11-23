# Validate saved query results<a name="cloudtrail-query-results-validation-intro"></a>

To determine whether the query results were modified, deleted, or unchanged after CloudTrail delivered the query results, you can use CloudTrail query results integrity validation\. This feature is built using industry standard algorithms: SHA\-256 for hashing and SHA\-256 with RSA for digital signing\. This makes it computationally infeasible to modify, delete or forge CloudTrail query result files without detection\. You can use the command line to validate the query result files\. 

## Why use it?<a name="cloudtrail-query-results-validation-intro-use-cases"></a>

Validated query result files are invaluable in security and forensic investigations\. For example, a validated query result file enables you to assert positively that the query result file itself has not changed\. The CloudTrail query result file integrity validation process also lets you know if a query result file has been deleted or changed\. 

**Topics**
+ [Why use it?](#cloudtrail-query-results-validation-intro-use-cases)
+ [Validate query results with the command line](cloudtrail-query-results-validation-cli.md)
+ [CloudTrail sign file structure](cloudtrail-results-file-validation-sign-file-structure.md)
+ [Custom implementations of CloudTrail query result file integrity validation](cloudtrail-results-file-custom-validation.md)