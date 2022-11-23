# Services that support TLS details in CloudTrail<a name="cloudtrail-supported-tls-details"></a>

AWS is updating the TLS configuration for all AWS service API endpoints to a minimum of TLS version 1\.2\. For more information, see the blog post, [TLS 1\.2 to become the minimum TLS protocol level for all AWS API endpoints](http://aws.amazon.com/blogs/security/tls-1-2-required-for-aws-endpoints/)\. The `tlsDetails` structure in each CloudTrail record contains the TLS version, cipher suite, and the client\-provided host name used in the service API call, which is typically the fully qualified domain name \(FQDN\) of the service endpoint\. You can then use the data in the records to help you pinpoint your client software that is responsible for the TLS 1\.0 or 1\.1 call, and update it accordingly\. Nearly half of AWS services currently provide the TLS information in the CloudTrail `tlsDetails` field\. The following table shows AWS services that display TLS information in CloudTrail records\.


| Services that support TLS details | 
| --- | 
| Alexa for Business | 
| AWS Activate | 
| AWS App Mesh | 
| Amazon AppStream 2\.0 | 
| AWS Auto Scaling | 
| AWS Backup Gateway | 
| AWS Billing | 
| AWS Certificate Manager | 
| AWS Cloud9 | 
| Amazon Cloud Directory | 
| AWS CloudFormation | 
| Amazon CloudFront | 
| AWS Cloud Map | 
| Amazon CloudSearch | 
| AWS CloudTrail | 
| Amazon CloudWatch | 
| Amazon CloudWatch Application Insights | 
| Amazon CloudWatch Events | 
| Amazon CloudWatch Logs | 
| AWS CodeArtifact | 
| AWS CodeBuild | 
| AWS CodeCommit | 
| AWS CodeDeploy | 
| AWS CodePipeline | 
| AWS CodeStar | 
| AWS CodeStar Connections | 
| Amazon Comprehend | 
| Amazon Comprehend Medical | 
| AWS Compute Optimizer | 
| Amazon Connect Voice ID | 
| AWS Control Tower | 
| AWS Cost and Usage Report | 
| AWS Cost Explorer | 
| AWS Database Migration Service \(DMS\) | 
| AWS DeepRacer | 
| AWS Device Farm | 
| AWS Diode | 
| AWS Direct Connect | 
| AWS Directory Service | 
| Amazon DynamoDB | 
| Amazon DynamoDB Accelerator \(DAX\) | 
| Amazon Elastic Block Store \(EBS\) | 
| Amazon Elastic Compute Cloud \(EC2\) | 
| Amazon EC2 Instance Connect | 
| Amazon Elastic Container Registry \(ECR\) | 
| Amazon Elastic Container Registry \(ECR\) Public | 
| Amazon Elastic Container Service \(ECS\) | 
| Amazon ElastiCache | 
| Amazon Elastic File System \(EFS\) | 
| Amazon Elastic Transcoder | 
| AWS Elastic Load Balancing \(ELB\) | 
| AWS Elastic Load Balancing \(ELBV2\) | 
| AWS Elemental MediaStore | 
| Amazon EMR | 
| Amazon EventBridge | 
| AWS Firewall Manager | 
| Amazon Fraud Detector | 
| Amazon FSx | 
| Amazon GameLift | 
| Amazon HealthLake | 
| AWS Identity and Access Management \(IAM\) | 
| Amazon Inspector | 
| AWS IoT Analytics | 
| AWS IoT Events | 
| Amazon Kendra | 
| AWS Key Management Service \(KMS\) | 
| Amazon Kinesis | 
| Amazon Kinesis Data Analytics | 
| Amazon Kinesis Data Firehose | 
| Amazon Kinesis Data Streams | 
| Amazon Kinesis Video Streams | 
| AWS License Manager | 
| Amazon Lookout for Equipment | 
| Amazon Machine Learning | 
| AWS Managed Services | 
| AWS Marketplace Commerce Analytics | 
| AWS Marketplace Discovery | 
| AWS Marketplace Entitlement Service | 
| AWS Marketplace Metering Service | 
| Amazon MemoryDB for Redis | 
| AWS Network Firewall | 
| Amazon OpenSearch Service | 
| AWS OpsWorks CM | 
| AWS Organizations | 
| Amazon Polly | 
| AWS Price List | 
| AWS Proton | 
| Amazon Redshift | 
| Amazon Rekognition | 
| Amazon Relational Database Service \(RDS\) | 
| AWS Resource Groups Tagging | 
| Amazon Route 53 | 
| Amazon Route 53 Resolver | 
| Amazon SageMaker | 
| Amazon SageMaker\-Edge | 
| AWS Secrets Manager | 
| AWS Security Token Service \(STS\) | 
| AWS Server Migration Service \(SMS\) | 
| AWS Service Catalog | 
| AWS Service Quotas | 
| AWS Shield | 
| Amazon SimpleDB | 
| Amazon Simple Notification Service \(SNS\) | 
| Amazon Simple Queue Service \(SQS\) | 
| Amazon Simple Storage Service \(S3\) | 
| Amazon S3 Glacier | 
| Amazon Simple Workflow Service \(SWF\) | 
| AWS Snowball | 
| AWS Step Functions | 
| AWS Storage Gateway | 
| AWS Support | 
| Amazon Textract | 
| Amazon Timestream | 
| Amazon Transcribe Streaming Service | 
| AWS Transfer Family | 
| Amazon Translate | 
| AWS Trusted Advisor | 
| Amazon Mechanical Turk | 
| AWS WAF | 
| Amazon WorkDocs | 
| Amazon WorkMail | 
| Amazon WorkMail Message Flow | 
| Amazon WorkSpaces | 
| AWS X\-Ray | 