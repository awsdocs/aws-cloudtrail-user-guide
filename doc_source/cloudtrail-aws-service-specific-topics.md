# CloudTrail Supported Services and Integrations<a name="cloudtrail-aws-service-specific-topics"></a>

CloudTrail supports logging events for many AWS services\. You can find the specifics for each supported service in that service's guide\. Links to those service\-specific topics are provided below\. In addition, some AWS services can be used to analyze and act upon data collected in CloudTrail logs\. You can browse an overview of those service integrations here\.

**Note**  
To see the list of supported regions for each service, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.


+ [AWS Service Integrations With CloudTrail Logs](#cloudtrail-aws-service-specific-topics-integrations)
+ [AWS Service Topics for CloudTrail](#cloudtrail-aws-service-specific-topics-list)
+ [CloudTrail Unsupported Services](cloudtrail-unsupported-aws-services.md)

## AWS Service Integrations With CloudTrail Logs<a name="cloudtrail-aws-service-specific-topics-integrations"></a>

You can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following topics\.


****  

| AWS Service | Topic | Description | 
| --- | --- | --- | 
| Amazon Athena | [Querying AWS CloudTrail Logs](http://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html) | Using Athena with CloudTrail logs is a powerful way to enhance your analysis of AWS service activity\. For example, you can use queries to identify trends and further isolate activity by attribute, such as source IP address or user\. You can automatically create tables for querying logs directly from the CloudTrail console, and use those tables to run queries in Athena\. For more information, see [Creating a Table for CloudTrail Logs in the CloudTrail Console](http://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html#create-cloudtrail-table-ct) in the [Amazon Athena User Guide](http://docs.aws.amazon.com/athena/latest/ug/)\. Running queries in Amazon Athena incurs additional costs\. For more information, see [Amazon Athena Pricing\.](https://aws.amazon.com/athena/pricing/) | 
| Amazon CloudWatch Logs | [Monitoring CloudTrail Log Files with Amazon CloudWatch Logs](monitor-cloudtrail-log-files-with-cloudwatch-logs.md) | You can configure CloudTrail with CloudWatch Logs to monitor your trail logs and be notified when specific activity occurs\. For example, you can define CloudWatch Logs metric filters that will trigger CloudWatch alarms and send notifications to you when those alarms are triggered\.   Standard pricing for Amazon CloudWatch and Amazon CloudWatch Logs applies\. For more information, see [Amazon CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)\.   | 

## AWS Service Topics for CloudTrail<a name="cloudtrail-aws-service-specific-topics-list"></a>

You can learn more about how the events for individual AWS services are recorded in CloudTrail logs, including example events for that service in log files\. For more information about how specific AWS services integrate with CloudTrail, see the topic about integration in the individual guide for that service\.


****  

| AWS Service | CloudTrail Topics | Support began | 
| --- | --- | --- | 
| Alexa for Business | [Logging Alexa for Business Administration Calls Using AWS CloudTrail](http://docs.aws.amazon.com/a4b/latest/ag/cloudtrail.html) | 11/29/2017 | 
| Amazon API Gateway | [Log API management calls to Amazon API Gateway Using AWS CloudTrail](http://docs.aws.amazon.com/apigateway/latest/developerguide/cloudtrail.html) | 07/09/2015 | 
| Application Auto Scaling | [Logging Application Auto Scaling API calls with AWS CloudTrail](http://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/logging-using-cloudtrail.html) | 10/31/2016 | 
| AWS Application Discovery Service | [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/) | 05/12/2016 | 
| AWS AppSync | [Logging AWS AppSync API Calls with AWS CloudTrail](http://docs.aws.amazon.com/appsync/latest/devguide/cloudtrail-logging.html) | 02/13/2018 | 
| Amazon Athena | [Logging Amazon Athena API Calls with AWS CloudTrail](http://docs.aws.amazon.com/athena/latest/ug/monitor-with-cloudtrail.html) | 05/19/2017 | 
| Amazon EC2 Auto Scaling | [Logging Auto Scaling API Calls By Using CloudTrail](http://docs.aws.amazon.com/autoscaling/latest/userguide/logging-using-cloudtrail.html) | 07/16/2014 | 
| AWS Batch | [Logging AWS Batch API Calls with AWS CloudTrail](http://docs.aws.amazon.com/batch/latest/userguide/logging-using-cloudtrail.html) | 1/10/2018 | 
| AWS Billing and Cost Management | [Logging AWS Billing and Cost Management API Calls with AWS CloudTrail](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/logging-using-cloudtrail.html) | 06/07/2018 | 
| AWS Certificate Manager | [Using AWS CloudTrail](http://docs.aws.amazon.com/acm/latest/userguide/cloudtrail.html) | 03/25/2016 | 
| Amazon Chime | [Log Amazon Chime Administration Calls Using AWS CloudTrail](http://docs.aws.amazon.com/chime/latest/ag/cloudtrail.html) | 09/27/2017 | 
| Amazon Cloud Directory |  [Logging Cloud Directory API calls Using AWS CloudTrail](http://docs.aws.amazon.com/amazoncds/latest/APIReference/cloudtrail_logging.html) | 01/26/2017 | 
| AWS CloudFormation | [Logging AWS CloudFormation API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-api-logging-cloudtrail.html) | 04/02/2014 | 
| Amazon CloudFront | [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/logging_using_cloudtrail.html) | 05/28/2014 | 
| AWS CloudHSM | [Logging AWS CloudHSM API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cloudtrail_logging.html) | 01/08/2015 | 
| Amazon CloudSearch | [Logging Amazon CloudSearch Configuration Service Calls Using AWS CloudTrail](http://docs.aws.amazon.com/cloudsearch/latest/developerguide/logging-config-api-calls.html) | 10/16/2014 | 
| AWS CloudTrail | [AWS CloudTrail API Reference](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/) | 11/13/2013 | 
| Amazon CloudWatch | [Logging Amazon CloudWatch API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/logging_cw_api_calls.html)  | 04/30/2014 | 
| CloudWatch Events | [Logging Amazon CloudWatch Events API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/logging_cw_api_calls_cwe.html) | 01/16/2016 | 
| CloudWatch Logs | [Logging Amazon CloudWatch Logs API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/logging_cw_api_calls_cwl.html) | 03/10/2016 | 
| AWS CodeBuild | [Logging AWS CodeBuild API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codebuild/latest/userguide/cloudtrail.html) | 12/01/2016 | 
| AWS CodeCommit | [Logging AWS CodeCommit API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html) | 01/11/2017 | 
| AWS CodeDeploy | [Monitoring Deployments with AWS CloudTrail](http://docs.aws.amazon.com/codedeploy/latest/userguide/cloudtrail-integ.html) | 12/16/2014 | 
| AWS CodePipeline | [Logging AWS CodePipeline API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/codepipeline/latest/userguide/integ-cloudtrail.html) | 07/09/2015 | 
| AWS CodeStar | [Logging AWS CodeStar API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codestar/latest/userguide/cloudtrail.html) | 06/14/2017 | 
| Amazon Cognito | [Logging Amazon Cognito API Calls with AWS CloudTrail](http://docs.aws.amazon.com/cognito/latest/developerguide/logging-using-cloudtrail.html) | 02/18/2016 | 
| AWS Config | [Logging AWS Config API Calls By with AWS CloudTrail](http://docs.aws.amazon.com/config/latest/developerguide/log-api-calls.html) | 02/10/2015 | 
| AWS Data Pipeline | [Logging AWS Data Pipeline API Calls by using AWS CloudTrail](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-cloudtrail-logging.html)  | 12/02/2014 | 
| AWS Database Migration Service \(AWS DMS\) | [Logging AWS Database Migration Service API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/dms/latest/userguide/CHAP_Monitoring.CloudTrail.html)  | 02/04/2016 | 
| AWS Device Farm | [Logging AWS Device Farm API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/devicefarm/latest/developerguide/cloudtrail.html) | 07/13/2015 | 
| AWS Direct Connect | [Logging AWS Direct Connect API Calls in AWS CloudTrail](http://docs.aws.amazon.com/directconnect/latest/UserGuide/logging_dc_api_calls.html) | 03/08/2014 | 
| AWS Directory Service | [Logging AWS Directory Service API Calls by Using CloudTrail](http://docs.aws.amazon.com/directoryservice/latest/devguide/cloudtrail_logging.html) | 05/14/2015 | 
| Amazon DynamoDB | [Logging DynamoDB Operations By Using AWS CloudTrail](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html) | 05/28/2015 | 
| Amazon Elastic Container Registry \(Amazon ECR\) | [ Logging Amazon ECR API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonECR/latest/userguide/logging-using-cloudtrail.html) | 12/21/2015 | 
| Amazon Elastic Container Service \(Amazon ECS\) | [ Logging Amazon ECS API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/logging-using-cloudtrail.html) | 04/09/2015 | 
| Amazon EC2 Systems Manager \(SSM\) | [Auditing SSM API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/ssm/latest/APIReference/logging-using-cloudtrail.html) | 11/13/2013 | 
| AWS Elastic Beanstalk \(Elastic Beanstalk\) | [Using Elastic Beanstalk API Calls with AWS CloudTrail](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudtrail.html) | 03/31/2014 | 
| Amazon Elastic Block Store \(Amazon EBS\) | [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html) | 11/13/2013 | 
| Amazon Elastic Compute Cloud \(Amazon EC2\) | [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html) | 11/13/2013 | 
| Amazon Elastic File System \(Amazon EFS\) | [Logging Amazon EFS API Calls with AWS CloudTrail](http://docs.aws.amazon.com/efs/latest/ug/logging-using-cloudtrail.html) | 06/28/2016 | 
| Amazon Elastic Container Service for Kubernetes \(Amazon EKS\) | [Logging Amazon EKS API Calls with AWS CloudTrail](http://docs.aws.amazon.com/eks/latest/userguide/logging-using-cloudtrail.html) | 06/05/2018 | 
| Elastic Load Balancing | [AWS CloudTrail Logging for Your Classic Load Balancer](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/ELB-API-Logs.html) and [AWS CloudTrail Logging for Your Application Load Balancer](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudtrail-logs.html) | 04/04/2014 | 
| Amazon Elastic Transcoder | [Logging Elastic Transcoder API Calls Using CloudTrail](http://docs.aws.amazon.com/elastictranscoder/latest/developerguide/logging_using_cloudtrail.html) | 10/27/2014 | 
| Amazon ElastiCache | [Logging Amazon ElastiCache API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Logging.html) | 09/15/2014 | 
| Amazon Elasticsearch Service | [Auditing Amazon Elasticsearch Service Domains with AWS CloudTrail](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html#es-managedomains-cloudtrailauditing) | 10/01/2015 | 
| AWS Elemental MediaConvert | [Logging AWS Elemental MediaConvert API Calls with CloudTrail](http://docs.aws.amazon.com/mediaconvert/latest/ug/logging-using-cloudtrail.html) | 11/27/2017 | 
| AWS Elemental MediaStore | [Logging AWS Elemental MediaStore API Calls with CloudTrail](http://docs.aws.amazon.com/mediastore/latest/ug/logging-using-cloudtrail.html) | 11/27/2017 | 
| Amazon EMR | [Logging Amazon EMR API Calls in AWS CloudTrail](http://docs.aws.amazon.com/emr/latest/ManagementGuide/logging_emr_api_calls.html) | 04/04/2014 | 
| Amazon GameLift | [Logging Amazon GameLift API Calls with AWS CloudTrail](http://docs.aws.amazon.com/gamelift/latest/developerguide/logging-using-cloudtrail.html) | 01/27/2016 | 
| Amazon Glacier | [Logging Amazon Glacier API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/amazonglacier/latest/dev/audit-logging.html) | 12/11/2014 | 
| AWS Glue | [Logging AWS Glue Operations Using AWS CloudTrail](http://docs.aws.amazon.com/glue/latest/dg/monitor-cloudtrail.html) | 11/07/2017 | 
| Amazon GuardDuty | [Logging Amazon GuardDuty API Calls with AWS CloudTrail](http://docs.aws.amazon.com/guardduty/latest/ug/logging-using-cloudtrail.html) | 02/12/2018 | 
| AWS Health | [Logging AWS Health API Calls with AWS CloudTrail](http://docs.aws.amazon.com/health/latest/ug/logging-using-cloudtrail.html) | 11/21/2016 | 
| AWS Identity and Access Management \(IAM\) | [Logging IAM Events with AWS CloudTrail](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html) | 11/13/2013 | 
| Amazon Inspector | [Logging Amazon Inspector API calls with AWS CloudTrail](http://docs.aws.amazon.com/inspector/latest/userguide/logging-using-cloudtrail.html) | 04/20/2016 | 
| AWS IoT | [Logging AWS IoT API Calls with AWS CloudTrail](http://docs.aws.amazon.com/iot/latest/developerguide/monitoring_overview.html#iot-using-cloudtrail) | 04/11/2016 | 
| AWS IoT Analytics | [Logging AWS IoT Analytics API calls with AWS CloudTrail](http://docs.aws.amazon.com/iotanalytics/latest/userguide/cloudtrail.html) | 04/23/2018 | 
| AWS Key Management Service \(AWS KMS\) | [Logging AWS KMS API Calls using AWS CloudTrail](http://docs.aws.amazon.com/kms/latest/developerguide/logging-using-cloudtrail.html) | 11/12/2014 | 
| Amazon Kinesis Data Firehose | [Monitoring Amazon Kinesis Data Firehose API Calls with AWS CloudTrail](http://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudtrail.html) | 03/17/2016 | 
| Amazon Kinesis Data Streams | [Logging Amazon Kinesis Data Streams API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/kinesis/latest/dev/logging_using_cloudtrail.html) | 04/25/2014 | 
| AWS Lambda |  [Logging AWS Lambda API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/lambda/latest/dg/logging-using-cloudtrail.html) [Using Lambda with AWS CloudTrail](http://docs.aws.amazon.com/lambda/latest/dg/wt-cloudtrail-events-adminuser.html)  |  Management events: 04/09/2015 Data events: 11/30/2017  | 
| Amazon Lex | [Logging Amazon Lex API Calls with CloudTrail](http://docs.aws.amazon.com/lex/latest/dg/monitoring-aws-lex-cloudtrail.html)  | 08/15/2017 | 
| Amazon Lightsail | [Logging Lightsail API Calls with AWS CloudTrail](https://lightsail.aws.amazon.com/ls/docs/how-to/article/logging-lightsail-api-calls-using-aws-cloudtrail) | 12/23/2016 | 
| Amazon Machine Learning |  [Logging Amazon ML API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/machine-learning/latest/dg/logging-using-cloudtrail.html)  | 12/10/2015 | 
| AWS Managed Services  | [AWS Managed Services](https://aws.amazon.com/managed-services) | 12/21/2016 | 
| AWS Marketplace | [AWS Marketplace Metering Service API Reference](http://docs.aws.amazon.com/marketplacemetering/latest/APIReference/Welcome.html) | 05/02/2017 | 
| AWS Migration Hub | [Logging AWS Migration Hub API Calls with AWS CloudTrail](http://docs.aws.amazon.com/migrationhub/latest/ug/logging-using-cloudtrail.html) | 08/14/2017 | 
| AWS OpsWorks | [Logging AWS OpsWorks API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/opsworks/latest/userguide/monitoring-cloudtrail.html) | 06/04/2014 | 
| AWS OpsWorks for Chef Automate | [Logging AWS OpsWorks for Chef Automate API Calls with AWS CloudTrail](http://docs.aws.amazon.com/opsworks/latest/userguide/logging-using-cloudtrail.html) | 11/23/2016 | 
| AWS Organizations |  [Logging AWS Organizations Events with AWS CloudTrail](http://docs.aws.amazon.com/organizations/latest/userguide/orgs_cloudtrail-integration.html) | 02/27/2017 | 
| AWS Personal Health Dashboard | [Logging AWS Health API Calls with AWS CloudTrail](http://docs.aws.amazon.com/health/latest/ug/logging-using-cloudtrail.html) | 12/01/2016 | 
| Amazon Pinpoint | [Logging Amazon Pinpoint API Calls with AWS CloudTrail](http://docs.aws.amazon.com/pinpoint/latest/developerguide/logging-using-cloudtrail.html) | 02/06/2018 | 
| Amazon Polly | [Logging Amazon Polly API Calls with AWS CloudTrail](http://docs.aws.amazon.com/polly/latest/dg/logging-using-cloudtrail.html) | 11/30/2016 | 
| AWS Private Certificate Authority \(PCA\) | [Using CloudTrail](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCtIntro.html) | 04/04/2018 | 
| Amazon QuickSight | [Logging Operations with CloudTrail](http://docs.aws.amazon.com/quicksight/latest/user/logging-using-cloudtrail.html) | 04/28/2017 | 
| Amazon Redshift | [Amazon Redshift API Reference](http://docs.aws.amazon.com/redshift/latest/APIReference/) | 06/10/2014 | 
| Amazon Rekognition | [Logging Amazon Rekognition API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/rekognition/latest/dg/logging-using-cloudtrail.html) | 04/6/2018 | 
| Amazon Relational Database Service \(Amazon RDS\) | [Logging Amazon RDS API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Auditing.html) | 11/13/2013 | 
| Route 53 | [Using AWS CloudTrail to Capture Requests Sent to the Route 53 API](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/logging-using-cloudtrail.html) | 02/11/2015 | 
| Amazon SageMaker |  [Logging Amazon SageMaker API Calls with AWS CloudTrail](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-using-cloudtrail.html)  | 01/11/2018 | 
| AWS Secrets Manager | [Monitor the Use of Your AWS Secrets Manager Secrets](http://docs.aws.amazon.com/secretsmanager/latest/userguide/monitoring.html#monitoring_cloudtrail) | 04/05/2018 | 
| AWS Security Token Service \(AWS STS\) |  [Logging IAM Events with AWS CloudTrail](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html) The IAM topic includes information for AWS STS\.  | 11/13/2013 | 
| AWS Server Migration Service | [AWS SMS API Reference](http://docs.aws.amazon.com/ServerMigration/latest/APIReference/) | 11/14/2016 | 
| AWS Service Catalog | [Logging AWS Service Catalog API Calls with AWS CloudTrail](http://docs.aws.amazon.com/servicecatalog/latest/dg/logging-using-cloudtrail.html) | 07/06/2016 | 
| AWS Shield | [Logging Shield Advanced API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-shield-using-cloudtrail.html) | 02/08/2018 | 
| Amazon Simple Email Service \(Amazon SES\) | [Logging Amazon SES API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/logging-using-cloudtrail.html) | 05/07/2015 | 
| Amazon Simple Notification Service \(Amazon SNS\) | [Logging Amazon Simple Notification Service API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/sns/latest/dg/logging-using-cloudtrail.html) | 10/09/2014 | 
| Amazon Simple Queue Service \(Amazon SQS\) | [Logging Amazon SQS API Actions Using AWS CloudTrail](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/logging-using-cloudtrail.html) | 07/16/2014 | 
| Amazon Simple Storage Service | [Logging Amazon S3 API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonS3/latest/dev/cloudtrail-logging.html) | Management events: 09/01/2015 Data events: 11/21/2016 | 
| Amazon Simple Workflow Service \(Amazon SWF\) | [Logging Amazon Simple Workflow Service API Calls with AWS CloudTrail](http://docs.aws.amazon.com/amazonswf/latest/developerguide/ct-logging.html) | 05/13/2014 | 
| AWS Single Sign\-On \(AWS SSO\) | [Logging AWS SSO API Calls with AWS CloudTrail](http://docs.aws.amazon.com/singlesignon/latest/userguide/logging-using-cloudtrail.html) | 12/07/2017 | 
| AWS Step Functions | [Logging AWS Step Functions API Calls with AWS CloudTrail](http://docs.aws.amazon.com/step-functions/latest/dg/cloud-trail.html) | 12/01/2016 | 
| AWS Storage Gateway |  [Logging AWS Storage Gateway API Calls by Using AWS CloudTrail](http://docs.aws.amazon.com/storagegateway/latest/userguide/logging-using-cloudtrail.html)  | 12/16/2014 | 
| AWS Support |  [Logging AWS Support API Calls with AWS CloudTrail](http://docs.aws.amazon.com/awssupport/latest/user/logging-using-cloudtrail.html)  | 04/21/2016 | 
| Amazon Virtual Private Cloud \(Amazon VPC\) |  [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html) The Amazon VPC API is a subset of the Amazon EC2 API\.   | 11/13/2013 | 
| AWS WAF | [Logging AWS WAF API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-using-cloudtrail.html) | 04/28/2016 | 
| Amazon WorkDocs | [Logging Amazon WorkDocs API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/workdocs/latest/adminguide/cloudtrail_logging.html) | 08/27/2014 | 
| Amazon WorkMail | [Logging Amazon WorkMail API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/workmail/latest/adminguide/logging-using-cloudtrail.html) | 12/12/2017 | 
| Amazon WorkSpaces | [Logging Amazon WorkSpaces API Calls by Using CloudTrail](http://docs.aws.amazon.com/workspaces/latest/devguide/cloudtrail_logging.html) | 04/09/2015 | 
| AWS X\-Ray | [Logging AWS X\-Ray API Calls With CloudTrail](http://docs.aws.amazon.com/xray/latest/devguide/xray-api-cloudtrail.html) | 04/25/2018 | 