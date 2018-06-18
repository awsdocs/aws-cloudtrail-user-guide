# Services Supported by CloudTrail Event History<a name="view-cloudtrail-events-supported-services"></a>

You can look up event history in the supported regions for the following services\. You can look up event history for create, modify, and delete API calls\. 

**Note**  
Event history supports the following APIs\. If you're looking for a specific API call that doesn't appear in the event history, create a trail and check the log files in your S3 bucket\. For more information about logging for specific services see [CloudTrail Supported Services](cloudtrail-supported-services.md)\. 

To view API calls that are supported by the event history feature, choose a service from the following list\. 


+ [Alexa for Business APIs](#view-cloudtrail-events-supported-apis-a4b)
+ [Amazon API Gateway APIs](#view-cloudtrail-events-supported-apis-api-gateway)
+ [AWS Application Discovery Service APIs](#view-cloudtrail-events-supported-apis-app-ds)
+ [AWS AppSync APIs](#view-cloudtrail-events-supported-apis-apsy)
+ [Amazon Athena APIs](#view-cloudtrail-events-supported-apis-athena)
+ [AWS Batch APIs](#view-cloudtrail-events-supported-apis-batch)
+ [AWS Certificate Manager APIs](#view-cloudtrail-events-supported-apis-certificate-manager)
+ [AWS Certificate Manager Private Certificate Authority APIs](#view-cloudtrail-events-supported-apis-pca)
+ [Amazon Chime APIs](#view-cloudtrail-events-supported-apis-chime)
+ [Amazon Cloud Directory APIs](#view-cloudtrail-events-supported-apis-cloud-directory)
+ [AWS CloudFormation APIs](#view-cloudtrail-events-supported-apis-cloudformation)
+ [Amazon CloudFront APIs](#view-cloudtrail-events-supported-apis-cloudfront)
+ [AWS CloudHSM APIs](#view-cloudtrail-events-supported-apis-cloudhsm)
+ [Amazon CloudSearch APIs](#view-cloudtrail-events-supported-apis-cloudsearch)
+ [AWS CloudTrail APIs](#view-cloudtrail-events-supported-apis-cloudtrail)
+ [Amazon CloudWatch APIs](#view-cloudtrail-events-supported-apis-cloudwatch)
+ [AWS CodeBuild APIs](#view-cloudtrail-events-supported-apis-codebuild)
+ [AWS CodeCommit APIs](#view-cloudtrail-events-supported-apis-codecommit)
+ [AWS CodeDeploy APIs](#view-cloudtrail-events-supported-apis-codedeploy)
+ [AWS CodePipeline APIs](#view-cloudtrail-events-supported-apis-codepipeline)
+ [AWS CodeStar APIs](#view-cloudtrail-events-supported-apis-codestar)
+ [Amazon Cognito Identity APIs](#view-cloudtrail-events-supported-apis-cognito)
+ [Amazon Cognito Sync APIs](#view-cloudtrail-events-supported-apis-cognito-sync)
+ [Amazon Cognito User Pools APIs](#view-cloudtrail-events-supported-apis-cognito-your-user-pools)
+ [AWS Config APIs](#view-cloudtrail-events-supported-apis-config)
+ [AWS Data Pipeline APIs](#view-cloudtrail-events-supported-apis-datapipeline)
+ [AWS Database Migration Service APIs](#view-cloudtrail-events-supported-apis-dms)
+ [AWS Direct Connect APIs](#view-cloudtrail-events-supported-apis-directconnect)
+ [AWS Directory Service APIs](#view-cloudtrail-events-supported-apis-ads)
+ [Amazon DynamoDB APIs](#view-cloudtrail-events-supported-apis-dynamodb)
+ [Amazon DynamoDB Accelerator \(DAX\) APIs](#view-cloudtrail-events-supported-apis-dynamodb-accelerator)
+ [Amazon EC2 APIs](#view-cloudtrail-events-supported-apis-ec2)
+ [Amazon EC2 Auto Scaling APIs](#view-cloudtrail-events-supported-apis-autoscaling)
+ [Amazon Elastic Container Service APIs](#view-cloudtrail-events-supported-apis-ecs)
+ [Amazon Elastic Container Registry \(Amazon ECR\) APIs](#view-cloudtrail-events-supported-apis-ecr)
+ [AWS Elastic Beanstalk APIs](#view-cloudtrail-events-supported-apis-elasticbeanstalk)
+ [Amazon Elastic File System APIs](#view-cloudtrail-events-supported-apis-elastic-file-system)
+ [Elastic Load Balancing APIs](#view-cloudtrail-events-supported-apis-elasticloadbalancing)
+ [Amazon Elasticsearch Service APIs](#view-cloudtrail-events-supported-apis-es)
+ [AWS Elemental MediaStore APIs](#view-cloudtrail-events-supported-apis-ems)
+ [Amazon EMR APIs](#view-cloudtrail-events-supported-apis-elasticmapreduce)
+ [Amazon Elastic Transcoder APIs](#view-cloudtrail-events-supported-apis-elastictranscoder)
+ [Amazon ElastiCache APIs](#view-cloudtrail-events-supported-apis-elasticache)
+ [AWS Firewall Manager API Calls](#view-cloudtrail-events-supported-apis-fms)
+ [Amazon GameLift APIs](#view-cloudtrail-events-supported-apis-gamelift)
+ [Amazon Glacier APIs](#view-cloudtrail-events-supported-apis-glacier)
+ [AWS Glue APIs](#view-cloudtrail-events-supported-apis-glue)
+ [Amazon GuardDuty APIs](#view-cloudtrail-events-supported-apis-guardduty)
+ [AWS Identity and Access Management APIs](#view-cloudtrail-events-supported-apis-iam)
+ [AWS Import/Export APIs](#view-cloudtrail-events-supported-apis-import-export)
+ [Amazon Inspector APIs](#view-cloudtrail-events-supported-apis-inspector)
+ [AWS IoT APIs](#view-cloudtrail-events-supported-apis-iot)
+ [Amazon Kinesis Data Firehose APIs](#view-cloudtrail-events-supported-apis-kinesis-firehose)
+ [Amazon Kinesis Data Streams APIs](#view-cloudtrail-events-supported-apis-kinesis)
+ [AWS Lambda APIs](#view-cloudtrail-events-supported-apis-lambda)
+ [Amazon Lex APIs](#view-cloudtrail-events-supported-apis-lex)
+ [AWS Amazon Lightsail APIs](#view-cloudtrail-events-supported-apis-lightsail)
+ [AWS Managed Services APIs](#view-cloudtrail-events-supported-apis-managed-services)
+ [AWS Marketplace Metering Service APIs](#view-cloudtrail-events-supported-apis-marketplace-metering)
+ [AWS Migration Hub APIs](#view-cloudtrail-events-supported-apis-migration-hub)
+ [AWS OpsWorks APIs](#view-cloudtrail-events-supported-apis-opsworks)
+ [AWS Organizations Events](#view-cloudtrail-events-supported-apis-organizations)
+ [AWS Personal Health Dashboard APIs](#view-cloudtrail-events-supported-apis-phd)
+ [Amazon Pinpoint APIs](#view-cloudtrail-events-supported-apis-pin)
+ [Amazon Polly APIs](#view-cloudtrail-events-supported-apis-pol)
+ [Amazon QuickSight Events](#view-cloudtrail-events-supported-apis-quicksight)
+ [Amazon Redshift APIs](#view-cloudtrail-events-supported-apis-redshift)
+ [Amazon Rekognition APIs](#view-cloudtrail-events-supported-apis-rekognition)
+ [Amazon Relational Database Service APIs](#view-cloudtrail-events-supported-apis-rds)
+ [Amazon Route 53 APIs](#view-cloudtrail-events-supported-apis-route53)
+ [Amazon SageMaker APIs](#view-cloudtrail-events-supported-apis-sm)
+ [AWS Secrets Manager APIs](#view-cloudtrail-events-supported-apis-asm)
+ [AWS Server Migration Service APIs](#view-cloudtrail-events-supported-apis-sms)
+ [AWS Service Catalog APIs](#view-cloudtrail-events-supported-apis-service-catalog)
+ [AWS Shield APIs](#view-cloudtrail-events-supported-apis-shield)
+ [Amazon Simple Storage Service \(S3\) Bucket Level APIs](#view-cloudtrail-events-supported-apis-s3)
+ [Amazon Simple Workflow Service APIs](#view-cloudtrail-events-supported-apis-swf)
+ [AWS Single Sign\-On APIs](#view-cloudtrail-events-supported-apis-sso)
+ [AWS Step Functions APIs](#view-cloudtrail-events-supported-apis-step-functions)
+ [AWS Storage Gateway APIs](#view-cloudtrail-events-supported-apis-storage-gateway)
+ [AWS Support APIs](#view-cloudtrail-events-supported-apis-aws-support)
+ [AWS WAF APIs](#view-cloudtrail-events-supported-apis-waf)
+ [Amazon WorkMail APIs](#view-cloudtrail-events-supported-apis-wm)
+ [AWS X\-Ray APIs](#console-view-cloudtrail-events-supported-apis-xray)
+ [AWS Billing Events](#console-billing-events)
+ [AWS Console Sign\-in Events](#console-sign-in-events)

## Alexa for Business APIs<a name="view-cloudtrail-events-supported-apis-a4b"></a>


****  

|  | 
| --- |
| AssociateDeviceWithRoom | 
| AssociateSkillGroupWithRoom | 
| CreateEnrollmentToken | 
| CreateProfile | 
| CreateRoom | 
| CreateSkillGroup | 
| CreateUser | 
| DeleteProfile | 
| DeleteRoom | 
| DeleteRoomSkillParameter | 
| DeleteSkillGroup | 
| DeleteUser | 
| DisassociateDeviceFromRoom | 
| DisassociateSkillGroupFromRoom | 
| PutRoomSkillParameter | 
| RevokeInvitation | 
| SendInvitation | 
| StartDeviceSync | 
| TagResource | 
| UntagResource | 
| UpdateDevice | 
| UpdateProfile | 
| UpdateRoom | 
| UpdateSkillGroup | 

For more information, see [Logging Alexa for Business Administration Calls Using AWS CloudTrail](http://docs.aws.amazon.com/a4b/latest/ag/cloudtrail.html) in the *Alexa for Business Administration Guide*\.

## Amazon API Gateway APIs<a name="view-cloudtrail-events-supported-apis-api-gateway"></a>


****  

|  | 
| --- |
| CreateApiKey | 
| CreateAuthorizer | 
| CreateBasePathMapping | 
| CreateDeployment | 
| CreateDocumentationPart | 
| CreateDocumentationVersion | 
| CreateDomainName | 
| CreateModel | 
| CreateRequestValidator | 
| CreateResource | 
| CreateRestApi | 
| CreateStage | 
| CreateUsagePlan | 
| CreateUsagePlanKey | 
| CreateVpcLink | 
| DeleteApiKey | 
| DeleteAuthorizer | 
| DeleteBasePathMapping | 
| DeleteClientCertificate | 
| DeleteDeployment | 
| DeleteDocumentationPart | 
| DeleteDocumentationVersion | 
| DeleteDomainName | 
| DeleteGatewayResponse | 
| DeleteIntegration | 
| DeleteIntegrationResponse | 
| DeleteMethod | 
| DeleteMethodResponse | 
| DeleteModel | 
| DeleteRequestValidator | 
| DeleteResource | 
| DeleteRestApi | 
| DeleteStage | 
| DeleteUsagePlan | 
| DeleteUsagePlanKey | 
| DeleteVpcLink | 
| ExportApiKeys | 
| FlushStageAuthorizersCache | 
| FlushStageCache | 
| GenerateClientCertificate | 
| ImportApiKeys | 
| ImportDocumentationParts | 
| ImportRestApi | 
| PutGatewayResponse | 
| PutIntegration | 
| PutIntegrationResponse | 
| PutMethod | 
| PutMethodResponse | 
| PutRestApi | 
| UpdateAccount | 
| UpdateApiKey | 
| UpdateAuthorizer | 
| UpdateBasePathMapping | 
| UpdateClientCertificate | 
| UpdateDeployment | 
| UpdateDocumentationPart | 
| UpdateDocumentationVersion | 
| UpdateDomainName | 
| UpdateGatewayResponse | 
| UpdateIntegration | 
| UpdateIntegrationResponse | 
| UpdateMethod | 
| UpdateMethodResponse | 
| UpdateModel | 
| UpdateRequestValidator | 
| UpdateResource | 
| UpdateRestApi | 
| UpdateStage | 
| UpdateUsage | 
| UpdateUsagePlan | 
| UpdateVpcLink | 

For more information, see [Logging Amazon API Gateway API Calls with AWS CloudTrail](http://docs.aws.amazon.com/apigateway/latest/developerguide/cloudtrail.html) in the *API Gateway Developer Guide*\.

## AWS Application Discovery Service APIs<a name="view-cloudtrail-events-supported-apis-app-ds"></a>


****  

|  | 
| --- |
| CreateTags | 
| DeleteTags | 
| ExportConfigurations | 
| StartDataCollectionByAgentIds | 
| StopDataCollectionByAgentIds | 

For more information, see the [Application Discovery Service User Guide](http://docs.aws.amazon.com/application-discovery/latest/userguide/)\.

## AWS AppSync APIs<a name="view-cloudtrail-events-supported-apis-apsy"></a>


****  

|  | 
| --- |
| CreateApiKey | 
| CreateDataSource | 
| CreateGraphqlApi | 
| CreateResolver | 
| CreateType | 
| DeleteApiKey | 
| DeleteDataSource | 
| DeleteGraphqlApi | 
| DeleteResolver | 
| DeleteType | 
| StartSchemaCreation | 
| UpdateApiKey | 
| UpdateDataSource | 
| UpdateGraphqlApi | 
| UpdateResolver | 
| UpdateType | 

For more information, see [Logging AWS AppSync API Calls with AWS CloudTrail](http://docs.aws.amazon.com/appsync/latest/devguide/cloudtrail-logging.html) in the *AWS AppSync Developer Guide*\.

## Amazon Athena APIs<a name="view-cloudtrail-events-supported-apis-athena"></a>


****  

|  | 
| --- |
| CreateNamedQuery | 
| DeleteNamedQuery | 
| StartQueryExecution | 
| StopQueryExecution | 

For more information, see [Logging Amazon Athena API Calls with AWS CloudTrail](http://docs.aws.amazon.com/athena/latest/ug/monitor-with-cloudtrail.html) in the *Amazon Athena User Guide*\.

## AWS Batch APIs<a name="view-cloudtrail-events-supported-apis-batch"></a>


****  

|  | 
| --- |
| AddTags | 
| CreateBatchPrediction | 
| CreateDataSourceFromRDS | 
| CreateDataSourceFromRedshift | 
| CreateDataSourceFromS3 | 
| CreateEvaluation | 
| CreateMLModel | 
| CreateRealtimeEndpoint | 
| DeleteBatchPrediction | 
| DeleteDataSource | 
| DeleteEvaluation | 
| DeleteMLModel | 
| DeleteRealtimeEndpoint | 
| DeleteTags | 
| UpdateBatchPrediction | 
| UpdateDataSource | 
| UpdateEvaluation | 
| UpdateMLModel | 

For more information, see [Logging AWS Batch API Calls with AWS CloudTrail](http://docs.aws.amazon.com/batch/latest/userguide/logging-using-cloudtrail.html) in the *AWS Batch User Guide*\.

## AWS Certificate Manager APIs<a name="view-cloudtrail-events-supported-apis-certificate-manager"></a>


****  

|  | 
| --- |
| AddTagsToCertificate | 
| DeleteCertificate | 
| RemoveTagsFromCertificate | 
| RequestCertificate | 
| ResendValidationEmail | 

For more information, see [Using AWS CloudTrail](http://docs.aws.amazon.com/acm/latest/userguide/cloudtrail.html) in the *AWS Certificate Manager User Guide*\.

## AWS Certificate Manager Private Certificate Authority APIs<a name="view-cloudtrail-events-supported-apis-pca"></a>


****  

|  | 
| --- |
| CreateCertificateAuthority | 
| CreateCertificateAuthorityAuditReport | 
| DeleteCertificateAuthority | 
| ImportCertificateAuthorityCertificate | 
| IssueCertificate | 
| RevokeCertificate | 
| TagCertificateAuthority | 
| UntagCertificateAuthority | 
| UpdateCertificateAuthority | 

For more information, see [Using CloudTrail](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCtIntro.html) in the *AWS Certificate Manager Private Certificate Authority User Guide*\.

## Amazon Chime APIs<a name="view-cloudtrail-events-supported-apis-chime"></a>


****  

|  | 
| --- |
| AcceptDelegate | 
| ActivateUsers | 
| AddDomain | 
| AddOrUpdateGroups | 
| AuthorizeDirectory | 
| ConnectDirectory | 
| CreateAccount | 
| CreateCDRBucket | 
| DeleteAccount | 
| DeleteCDRBucket | 
| DeleteDelegate | 
| DeleteDomain | 
| DeleteGroups | 
| DisconnectDirectory | 
| InviteDelegate | 
| InviteUsers | 
| LogoutUser | 
| RenameAccount | 
| RenewDelegate | 
| ResetAccountResource | 
| ResetPersonalPIN | 
| SubmitSupportRequest | 
| SuspendUsers | 
| UnauthorizeDirectory | 
| UpdateAccountResource | 
| UpdateAccountSettings | 
| UpdateCDRSettings | 
| UpdateSupportedLicenses | 
| UpdateUserLicenses | 
| ValidateAccountResource | 
| ValidateDelegate | 

For more information, see [Logging Amazon Chime Administration Calls with AWS CloudTrail](http://docs.aws.amazon.com/chime/latest/ag/cloudtrail.html) in the *Amazon Chime Administrator Guide*\.

## Amazon Cloud Directory APIs<a name="view-cloudtrail-events-supported-apis-cloud-directory"></a>


****  

|  | 
| --- |
| ApplySchema | 
| CreateDirectory | 
| CreateFacet | 
| CreateSchema | 
| CreateTypedLinkFacet | 
| DeleteDirectory | 
| DeleteFacet | 
| DeleteSchema | 
| DeleteTypedLinkFacet | 
| DisableDirectory | 
| EnableDirectory | 
| PublishSchema | 
| PutSchemaFromJson | 
| UpdateFacet | 
| UpdateSchema | 
| UpdateTypedLinkFacet | 
| UpgradeAppliedSchema | 
| UpgradePublishedSchema | 

For more information, see [Logging Amazon Cloud Directory API Calls with AWS CloudTrail](http://docs.aws.amazon.com/directoryservice/latest/APIReference/cloudtrail_logging.html) in the *Amazon Cloud Directory API Reference*\.

## AWS CloudFormation APIs<a name="view-cloudtrail-events-supported-apis-cloudformation"></a>


****  

|  | 
| --- |
| CancelUpdateStack | 
| CreateChangeSet | 
| CreateStack | 
| CreateStackInstances | 
| CreateStackSet | 
| ContinueUpdateRollback | 
| DeleteChangeSet | 
| DeleteStack | 
| DeleteStackInstances | 
| DeleteStackSet | 
| ExecuteChangeSet | 
| SetStackPolicy | 
| SignalResource | 
| StopStackSetOperation | 
| UpdateStack | 
| UpdateStackSet | 
| UpdateTerminationProtection | 

For more information, see [Logging AWS CloudFormation API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-api-logging-cloudtrail.html) in the *AWS CloudFormation User Guide*\.

## Amazon CloudFront APIs<a name="view-cloudtrail-events-supported-apis-cloudfront"></a>


****  

|  | 
| --- |
| CreateCloudFrontOriginAccessIdentity | 
| CreateDistribution | 
| CreateInvalidation | 
| CreateStreamingDistribution | 
| DeleteCloudFrontOriginAccessIdentity | 
| DeleteDistribution | 
| DeleteStreamingDistribution | 
| UpdateCloudFrontOriginAccessIdentity | 
| UpdateDistribution | 
| UpdateStreamingDistribution | 

For more information, see [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/logging_using_cloudtrail.html) in the *Amazon CloudFront Developer Guide*\.

## AWS CloudHSM APIs<a name="view-cloudtrail-events-supported-apis-cloudhsm"></a>


****  

|  | 
| --- |
| AdminCreateHsm | 
| CreateCluster | 
| CreateHapg | 
| CreateHsm | 
| CreateLunaClient | 
| DeleteCluster | 
| DeleteHapg | 
| DeleteHsm | 
| DeleteLunaClient | 
| InitializeCluster | 
| ModifyHapg | 
| ModifyHsm | 
| ModifyLunaClient | 
| TagResource | 
| UntagResource | 

For more information, see [Logging AWS CloudHSM API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cloudtrail_logging.html) in the *AWS CloudHSM User Guide*\.

## Amazon CloudSearch APIs<a name="view-cloudtrail-events-supported-apis-cloudsearch"></a>


****  

|  | 
| --- |
| BuildSuggesters | 
| CreateDomain | 
| DefineAnalysisScheme | 
| DefineExpression | 
| DefineIndexField | 
| DefineIndexFields | 
| DefineRankExpression | 
| DefineSuggester | 
| DeleteAnalysisScheme | 
| DeleteDomain | 
| DeleteExpression | 
| DeleteIndexField | 
| DeleteRankExpression | 
| DeleteSuggester | 
| IndexDocuments | 
| UpdateAvailabilityOptions | 
| UpdateDefaultSearchField | 
| UpdateScalingParameters | 
| UpdateServiceAccessPolicies | 
| UpdateStemmingOptions | 
| UpdateStopwordOptions | 
| UpdateSynonymOptions | 

For more information, see [Logging Amazon CloudSearch Configuration Service Calls Using AWS CloudTrail](http://docs.aws.amazon.com/cloudsearch/latest/developerguide/logging-config-api-calls.html) in the *Amazon CloudSearch Developer Guide*\.

## AWS CloudTrail APIs<a name="view-cloudtrail-events-supported-apis-cloudtrail"></a>


****  

|  | 
| --- |
| AddTags | 
| CreateTrail | 
| DeleteTrail | 
| PutEventSelectors | 
| RemoveTags | 
| StartLogging | 
| StopLogging | 
| UpdateTrail | 

## Amazon CloudWatch APIs<a name="view-cloudtrail-events-supported-apis-cloudwatch"></a>

**Amazon CloudWatch**


****  

|  | 
| --- |
| DeleteAlarms | 
| DisableAlarmActions | 
| EnableAlarmActions | 
| PutMetricAlarm | 
| SetAlarmState | 

**Amazon CloudWatch Events**


****  

|  | 
| --- |
| DeleteRule | 
| DisableRule | 
| EnableRule | 
| PutRule | 
| PutTargets | 
| RemoveTargets | 

**Amazon CloudWatch Logs**


****  

|  | 
| --- |
| CancelExportTask | 
| CreateExportTask | 
| CreateLogGroup | 
| CreateLogStream | 
| DeleteDestination | 
| DeleteLogGroup | 
| CreateLogStream | 
| DeleteMetricFilter | 
| DeleteRetentionPolicy | 
| DeleteSubscriptionFilter | 
| PutDestination | 
| PutDestinationPolicy | 
| PutMetricFilter | 
| PutRetentionPolicy | 
| PutSubscriptionFilter | 

For more information, see [Logging Amazon CloudWatch API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/logging_cw_api_calls.html) in the *Amazon CloudWatch User Guide*\.

## AWS CodeBuild APIs<a name="view-cloudtrail-events-supported-apis-codebuild"></a>


****  

|  | 
| --- |
| BatchDeleteBuilds | 
| BatchGetProjects | 
| CreateProject | 
| CreateWebhook | 
| DeleteProject | 
| DeleteWebhook | 
| ProcessWebhook | 
| StartBuild | 
| StopBuild | 
| UpdateProject | 

 For information about the AWS CodeBuild calls logged by CloudTrail, see [Logging AWS CodeBuild API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codebuild/latest/userguide/cloudtrail.html) in the *AWS CodeBuild User Guide*\.

## AWS CodeCommit APIs<a name="view-cloudtrail-events-supported-apis-codecommit"></a>


****  

|  | 
| --- |
| CreateBranch | 
| CreateRepository | 
| DeleteRepository | 
| PutRepositoryTriggers | 
| TestRepositoryTriggers | 
| UpdateDefaultBranch | 
| UpdateRepositoryDescription | 
| UpdateRepositoryName | 

 For information about the AWS CodeCommit calls logged by CloudTrail, see [Logging AWS CodeCommit API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html) in the *AWS CodeCommit User Guide*\.

## AWS CodeDeploy APIs<a name="view-cloudtrail-events-supported-apis-codedeploy"></a>


****  

|  | 
| --- |
| AddTagsToOnPremisesInstances | 
| BindGitHubAccountTokenToApplication | 
| ContinueDeployment | 
| CreateApplication | 
| CreateDeployment | 
| CreateDeploymentConfig | 
| CreateDeploymentGroup | 
| DeleteApplication | 
| DeleteDeploymentConfig | 
| DeleteDeploymentGroup | 
| DeleteGitHubAccountToken | 
| DeregisterOnPremisesInstance | 
| RegisterApplicationRevision | 
| RegisterOnPremisesInstance | 
| RemoveTagsFromOnPremisesInstances | 
| SkipWaitTimeForInstanceTermination | 
| StopDeployment | 
| UpdateApplication | 
| UpdateDeploymentGroup | 

For more information, see [Monitoring Deployments with AWS CloudTrail](http://docs.aws.amazon.com/codedeploy/latest/userguide/cloudtrail-integ.html) in the *AWS CodeDeploy User Guide*\.

## AWS CodePipeline APIs<a name="view-cloudtrail-events-supported-apis-codepipeline"></a>


****  

|  | 
| --- |
| AcknowledgeJob | 
| AcknowledgeThirdPartyJob | 
| CreateCustomActionType | 
| CreatePipeline | 
| DeleteCustomActionType | 
| DeletePipeline | 
| DisableStageTransition | 
| EnableStageTransition | 
| PutActionRevision | 
| PutApprovalResult | 
| PutJobFailureResult | 
| PutJobSuccessResult | 
| PutThirdPartyJobFailureResult | 
| PutThirdPartyJobSuccessResult | 
| RetryStageExecution | 
| StartPipelineExecution | 
| UpdatePipeline | 

For more information,see [Logging AWS CodePipeline API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codepipeline/latest/userguide/integ-cloudtrail.html) in the *AWS CodePipeline User Guide*\.

## AWS CodeStar APIs<a name="view-cloudtrail-events-supported-apis-codestar"></a>


****  

|  | 
| --- |
| AssociateTeamMember | 
| CreateProject | 
| CreateUserProfile | 
| DisassociateTeamMember | 
| DeleteProject | 
| DeleteUserProfile | 
| TagProject | 
| UntagProject | 
| UpdateProject | 
| UpdateUpserProfile | 
| UpdateTeamMember | 

For more information,see [Logging AWS CodeStar API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codestar/latest/userguide/cloudtrail.html) in the *AWS CodeStar User Guide*\.

## Amazon Cognito Identity APIs<a name="view-cloudtrail-events-supported-apis-cognito"></a>


****  

|  | 
| --- |
| CreateIdentityPool | 
| DeleteIdentities | 
| DeleteIdentityPool | 
| GetOpenIdTokenForDeveloperIdentity | 
| MergeDeveloperIdentities | 
| SetIdentityPoolRoles | 
| UnlinkDeveloperIdentity | 
| UpdateIdentityPool | 

For more information, see the [Logging Amazon Cognito API Calls with AWS CloudTrail](http://docs.aws.amazon.com/cognito/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon Cognito Developer Guide*\.

## Amazon Cognito Sync APIs<a name="view-cloudtrail-events-supported-apis-cognito-sync"></a>


****  

|  | 
| --- |
| SetCognitoEvents | 
| SetIdentityPoolConfiguration | 

For more information, see the [Logging Amazon Cognito API Calls with AWS CloudTrail](http://docs.aws.amazon.com/cognito/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon Cognito Developer Guide*\.

## Amazon Cognito User Pools APIs<a name="view-cloudtrail-events-supported-apis-cognito-your-user-pools"></a>


****  

|  | 
| --- |
| AddCustomAttributes | 
| AdminConfirmSignUp | 
| AdminDeleteUser | 
| AdminDeleteUserAttributes | 
| AdminDisableUser | 
| AdminRemoveUserFromGroup | 
| AdminResetUserPassword | 
| AdminSetUserSettings | 
| AdminUpdateUserAttributes | 
| CreateGroup | 
| CreateUserPool | 
| CreateUserPoolClient | 
| DeleteGroup | 
| DeleteUserPool | 
| DeleteUserPoolClient | 
| UpdateGroup | 
| UpdateUserPool | 
| UpdateUserPoolClient | 

For more information, see the [Logging Amazon Cognito API Calls with AWS CloudTrail](http://docs.aws.amazon.com/cognito/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon Cognito Developer Guide*\.

## AWS Config APIs<a name="view-cloudtrail-events-supported-apis-config"></a>


****  

|  | 
| --- |
| DeleteConfigRule | 
| DeleteConfigurationRecorder | 
| DeleteDeliveryChannel | 
| DeleteEvaluationResults | 
| PutConfigurationRecorder | 
| PutConfigRule | 
| PutDeliveryChannel | 
| PutEvaluations | 
| StartConfigRulesEvaluation | 
| StartConfigurationRecorder | 
| StopConfigurationRecorder | 

For more information, see [Logging AWS Config API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/config/latest/developerguide/log-api-calls.html) in the *AWS Config Developer Guide*\.

## AWS Data Pipeline APIs<a name="view-cloudtrail-events-supported-apis-datapipeline"></a>


****  

|  | 
| --- |
| ActivatePipeline | 
| AddTags | 
| CreatePipeline | 
| DeactivatePipeline | 
| DeletePipeline | 
| PutPipelineDefinition | 
| RemoveTags | 
| SetStatus | 

For more information, see [Logging AWS Data Pipeline API Calls by using AWS CloudTrail](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-cloudtrail-logging.html) in the *AWS Data Pipeline Developer Guide*\.

## AWS Database Migration Service APIs<a name="view-cloudtrail-events-supported-apis-dms"></a>


****  

|  | 
| --- |
| AddTagsToResource | 
| CreateEndpoint | 
| CreateEventSubscription | 
| CreateReplicationInstance | 
| CreateReplicationSubnetGroup | 
| CreateReplicationTask | 
| DeleteCertificate | 
| DeleteEndpoint | 
| DeleteEventSubscription | 
| DeleteReplicationInstance | 
| DeleteReplicationSubnetGroup | 
| DeleteReplicationTask | 
| ImportCertificate | 
| ModifyEndpoint | 
| ModifyEventSubscription | 
| ModifyReplicationInstance | 
| ModifyReplicationSubnetGroup | 
| ModifyReplicationTask | 
| RebootReplicationInstance | 
| RefreshSchemas | 
| ReloadTables | 
| RemoveTagsFromResource | 
| StartReplicationTask | 
| StartReplicationTaskAssessment | 
| StopReplicationTask | 

For more information, see [Logging AWS Database Migration Service API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/dms/latest/userguide/CHAP_Monitoring.CloudTrail.html) in the *AWS Database Migration Service User Guide*\.

## AWS Direct Connect APIs<a name="view-cloudtrail-events-supported-apis-directconnect"></a>


****  

|  | 
| --- |
| AcceptDirectConnectGatewayAssociationProposal | 
| AllocateConnectionOnInterconnect | 
| AllocateHostedConnection | 
| AllocatePrivateVirtualInterface | 
| AllocatePublicVirtualInterface | 
| AssociateConnectionWithLag | 
| AssociateHostedConnection | 
| AssociateVirtualInterface | 
| ConfirmConnection | 
| ConfirmPrivateVirtualInterface | 
| ConfirmPublicVirtualInterface | 
| CreateConnection | 
| CreateDirectConnectGateway | 
| CreateDirectConnectGatewayConnection | 
| CreateDirectConnectGatewayAssociationProposal | 
| CreateInterconnect | 
| CreateLag | 
| CreatePrivateVirtualInterface | 
| CreatePublicVirtualInterface | 
| DeleteConnection | 
| DeleteDirectConnectGateway | 
| DeleteDirectConnectGatewayAssociation | 
| DeleteDirectConnectGatewayAssociationProposal | 
| DeleteInterconnect | 
| DeleteLag | 
| DeleteVirtualInterface | 
| DisassociateConnectionFromLag | 
| UpdateLag | 

For more information, see [Logging AWS Direct Connect API Calls in AWS CloudTrail](http://docs.aws.amazon.com/directconnect/latest/UserGuide/logging_dc_api_calls.html) in the *AWS Direct Connect User Guide*\.

## AWS Directory Service APIs<a name="view-cloudtrail-events-supported-apis-ads"></a>


****  

|  | 
| --- |
| AddIpRoutes | 
| AddTagsToResource | 
| CancelSchemaExtension | 
| ConnectDirectory | 
| CreateAlias | 
| CreateComputer | 
| CreateConditionalForwarder | 
| CreateDirectory | 
| CreateMicrosoftAD | 
| CreateSnapshot | 
| CreateTrust | 
| DeleteConditionalForwarder | 
| DeleteDirectory | 
| DeleteSnapshot | 
| DeleteTrust | 
| DeregisterEventTopic | 
| DisableRadius | 
| DisableSso | 
| EnableRadius | 
| EnableSso | 
| RegisterEventTopic | 
| RemoveIpRoutes | 
| RemoveTagsFromResource | 
| RestoreFromSnapshot | 
| StartSchemaExtension | 
| UpdateConditionalForwarder | 
| UpdateNumberOfDomainControllers | 
| UpdateRadius | 

For more information, see [Logging AWS Directory Service API Calls by Using CloudTrail](http://docs.aws.amazon.com/directoryservice/latest/devguide/cloudtrail_logging.html) in the *AWS Directory Service Administration Guide*\.

## Amazon DynamoDB APIs<a name="view-cloudtrail-events-supported-apis-dynamodb"></a>


****  

|  | 
| --- |
| CreateTable | 
| DeleteTable | 
| PurchaseReservedCapacityOfferings | 
| UpdateTable | 

For more information, see [Logging DynamoDB Operations By Using AWS CloudTrail](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon DynamoDB Developer Guide*\.

## Amazon DynamoDB Accelerator \(DAX\) APIs<a name="view-cloudtrail-events-supported-apis-dynamodb-accelerator"></a>


****  

|  | 
| --- |
| CreateCluster | 
| CreateParameterGroup | 
| CreateSubnetGroup | 
| DecreaseReplicationFactor | 
| DeleteCluster | 
| DeleteParameterGroup | 
| DeleteSubnetGroup | 
| IncreaseReplicationFactor | 
| RebootNode | 
| TagResource | 
| UntagResource | 
| UpdateCluster | 
| UpdateParameterGroup | 
| UpdateSubnetGroup | 

For more information, see the [DAX API Reference](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.api.html)\.

## Amazon EC2 APIs<a name="view-cloudtrail-events-supported-apis-ec2"></a>


****  

|  | 
| --- |
| AcceptReservedInstancesExchangeQuote | 
| AcceptVpcPeeringConnection | 
| AllocateAddress | 
| AllocateHosts | 
| AssignIpv6Addresses | 
| AssignPrivateIpAddresses | 
| AssociateAddress | 
| AssociateDhcpOptions | 
| AssociateIamInstanceProfile | 
| AssociateRouteTable | 
| AssociateSubnetCidrBlock | 
| AssociateVpcCidrBlock | 
| AttachClassicLinkVpc | 
| AttachInternetGateway | 
| AttachNetworkInterface | 
| AttachVolume | 
| AttachVpnGateway | 
| AuthorizeSecurityGroupEgress | 
| AuthorizeSecurityGroupIngress | 
| BundleInstance | 
| CancelBundleTask | 
| CancelConversionTask | 
| CancelExportTask | 
| CancelImportTask | 
| CancelReservedInstancesListing | 
| CancelSpotInstanceRequests | 
| CopyImage | 
| CopySnapshot | 
| CreateCustomerGateway | 
| CreateDefaultSubnet | 
| CreateDefaultVpc | 
| CreateDhcpOptions | 
| CreateEgressOnlyInternetGateway  | 
| CreateImage | 
| CreateInstanceExportTask | 
| CreateInternetGateway | 
| CreateKeyPair | 
| CreateNatGateway | 
| CreateNetworkAcl | 
| CreateNetworkAclEntry | 
| CreateNetworkInterface | 
| CreatePlacementGroup | 
| CreateReservedInstancesListing | 
| CreateRoute | 
| CreateRouteTable | 
| CreateSecurityGroup | 
| CreateSnapshot | 
| CreateSpotDatafeedSubscription | 
| CreateSubnet | 
| CreateTags | 
| CreateVolume | 
| CreateVpc | 
| CreateVpcEndpoint | 
| CreateVpcPeeringConnection | 
| CreateVpnConnection | 
| CreateVpnConnectionRoute | 
| CreateVpnGateway | 
| DeleteCustomerGateway | 
| DeleteDhcpOptions | 
|  DeleteEgressOnlyInternetGateway  | 
| DeleteInternetGateway | 
| DeleteKeyPair | 
| DeleteNatGateway | 
| DeleteNetworkAcl | 
| DeleteNetworkAclEntry | 
| DeleteNetworkInterface | 
| DeletePlacementGroup | 
| DeleteRoute | 
| DeleteRouteTable | 
| DeleteSecurityGroup | 
| DeleteSnapshot | 
| DeleteSpotDatafeedSubscription | 
| DeleteSubnet | 
| DeleteTags | 
| DeleteVolume | 
| DeleteVpc | 
| DeleteVpcEndpoints | 
| DeleteVpcPeeringConnection | 
| DeleteVpnConnection | 
| DeleteVpnConnectionRoute | 
| DeleteVpnGateway | 
| DeregisterImage | 
| DetachClassicLinkVpc | 
| DetachInternetGateway | 
| DetachNetworkInterface | 
| DetachVolume | 
| DetachVpnGateway | 
| DisableVgwRoutePropagation | 
| DisableVpcClassicLink | 
| DisassociateAddress | 
| DisassociateIamInstanceProfile  | 
| DisassociateRouteTable | 
| DisassociateSubnetCidrBlock  | 
| DisassociateVpcCidrBlock  | 
| EnableDefaultTenancyForVpc | 
| EnableVgwRoutePropagation | 
| EnableVolumeIO | 
| EnableVpcClassicLink | 
| ImportImage | 
| ImportInstance | 
| ImportKeyPair | 
| ImportSnapshot | 
| ImportVolume | 
| ModifyHosts | 
| ModifyIdentityIdFormat | 
| ModifyImageAttribute | 
| ModifyInstanceAttribute | 
| ModifyInstancePlacement | 
| ModifyNetworkInterfaceAttribute | 
| ModifyReservedInstances | 
| ModifySnapshotAttribute | 
| ModifySubnetAttribute | 
| ModifyVolume | 
| ModifyVolumeAttribute | 
| ModifyVpcAttribute | 
| ModifyVpcEndpoint | 
| MonitorInstances | 
| MoveAddressToVpc | 
| PurchaseHostReservation | 
| PurchaseReservedInstancesOffering | 
| RebootInstances | 
| RegisterImage | 
| RejectVpcPeeringConnection | 
| ReleaseAddress | 
| ReleaseHosts | 
| ReplaceIamInstanceProfileAssociation | 
| ReplaceNetworkAclAssociation | 
| ReplaceNetworkAclEntry | 
| ReplaceRoute | 
| ReplaceRouteTableAssociation | 
| RequestSpotInstances | 
| ResetImageAttribute | 
| ResetInstanceAttribute | 
| ResetNetworkInterfaceAttribute | 
| ResetSnapshotAttribute | 
| RestoreAddressToClassic | 
| RevokeSecurityGroupEgress | 
| RevokeSecurityGroupIngress | 
| RunInstances | 
| StartInstances | 
| StopInstances | 
| TerminateInstances | 
| UnassignIpv6Addresses | 
| UnassignPrivateIpAddresses | 
| UnmonitorInstances | 

For more information, see [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html) in the *Amazon EC2 API Reference*\.

## Amazon EC2 Auto Scaling APIs<a name="view-cloudtrail-events-supported-apis-autoscaling"></a>


****  

|  | 
| --- |
| AttachInstances | 
| AttachLoadBalancers | 
| CompleteLifecycleAction | 
| CreateAutoScalingGroup | 
| CreateLaunchConfiguration | 
| CreateOrUpdateTags | 
| DeleteAutoScalingGroup | 
| DeleteLaunchConfiguration | 
| DeleteLifecycleHook | 
| DeleteNotificationConfiguration | 
| DeletePolicy | 
| DeleteScheduledAction | 
| DeleteTags | 
| DetachInstances | 
| DetachLoadBalancers | 
| DisableMetricsCollection | 
| EnableMetricsCollection | 
| EnterStandby | 
| ExecutePolicy | 
| ExitStandby | 
| PutLifecycleHook | 
| PutNotificationConfiguration | 
| PutScalingPolicy | 
| PutScheduledUpdateGroupAction | 
| RecordLifecycleActionHeartbeat | 
| ResumeProcesses | 
| SetDesiredCapacity | 
| SetInstanceHealth | 
| SetInstanceProtection | 
| SuspendProcesses | 
| TerminateInstanceInAutoScalingGroup | 
| UpdateAutoScalingGroup | 

For more information, see [Logging Auto Scaling API Calls By Using CloudTrail](http://docs.aws.amazon.com/autoscaling/latest/userguide/logging-using-cloudtrail.html) in the *Amazon EC2 Auto Scaling User Guide*\.

## Amazon Elastic Container Service APIs<a name="view-cloudtrail-events-supported-apis-ecs"></a>


****  

|  | 
| --- |
| RegisterTaskDefinition | 
| DeleteAttributes | 
| CreateCluster | 
| DeleteCluster | 
| RunTask | 
| UpdateContainerAgent | 
| StartTask | 
| UpdateContainerInstancesState | 
| StopTask | 
| CreateService | 
| SubmitContainerStateChange | 
| PutAttributes | 
| RegisterContainerInstance | 
| DeregisterTaskDefinition | 
| UpdateService | 
| DeleteService | 
| DeregisterContainerInstance | 

For more information, see [Logging Amazon ECS API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon Elastic Container Service Developer Guide*\.

## Amazon Elastic Container Registry \(Amazon ECR\) APIs<a name="view-cloudtrail-events-supported-apis-ecr"></a>


****  

|  | 
| --- |
| DryRunEvent | 
| PolicyExecutionEvent | 

For information about the Amazon ECR calls logged by CloudTrail, see [Logging Amazon ECR API Calls with AWS CloudTrail](http://docs.aws.amazon.com/AmazonECR/latest/userguide/logging-using-cloudtrail.html)\.

## AWS Elastic Beanstalk APIs<a name="view-cloudtrail-events-supported-apis-elasticbeanstalk"></a>


****  

|  | 
| --- |
| AbortEnvironmentUpdate | 
| CreateApplication | 
| CreateApplicationVersion | 
| CreateConfigurationTemplate | 
| CreateEnvironment | 
| CreateStorageLocation | 
| DeleteApplication | 
| DeleteApplicationVersion | 
| DeleteConfigurationTemplate | 
| DeleteEnvironmentConfiguration | 
| RebuildEnvironment | 
| RestartAppServer | 
| SwapEnvironmentCNAMEs | 
| TerminateEnvironment | 
| UpdateApplication | 
| UpdateApplicationVersion | 
| UpdateConfigurationTemplate | 
| UpdateEnvironment | 

For more information, see [Using AWS Elastic Beanstalk with AWS CloudTrail](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudtrail.html) in the *AWS Elastic Beanstalk Developer Guide*\.

## Amazon Elastic File System APIs<a name="view-cloudtrail-events-supported-apis-elastic-file-system"></a>


****  

|  | 
| --- |
| CreateFileSystem | 
| CreateMountTarget | 
| CreateTags | 
| DeleteFileSystem | 
| DeleteMountTarget | 
| DeleteTags | 
| ModifyMountTargetSecurityGroups | 

For information about the Amazon EFS calls logged by CloudTrail, see [Logging Amazon EFS API Calls with AWS CloudTrail](http://docs.aws.amazon.com/efs/latest/ug/logging-using-cloudtrail.html)\.

## Elastic Load Balancing APIs<a name="view-cloudtrail-events-supported-apis-elasticloadbalancing"></a>


****  

|  | 
| --- |
| AddTags | 
| ApplySecurityGroupsToLoadBalancer | 
| AttachLoadBalancerToSubnets | 
| ConfigureHealthCheck | 
| CreateAppCookieStickinessPolicy | 
| CreateLBCookieStickinessPolicy | 
| CreateListener | 
| CreateLoadBalancer | 
| CreateLoadBalancerListeners | 
| CreateLoadBalancerPolicy | 
| CreateRule | 
| CreateTargetGroup | 
| DeleteListener | 
| DeleteLoadBalancer | 
| DeleteLoadBalancerListeners | 
| DeleteLoadBalancerPolicy | 
| DeleteRule | 
| DeleteTargetGroup | 
| DeregisterInstancesFromLoadBalancer | 
| DeregisterTargets | 
| DetachLoadBalancerFromSubnets | 
| DisableAvailabilityZonesForLoadBalancer | 
| EnableAvailabilityZonesForLoadBalancer | 
| ModifyListener | 
| ModifyLoadBalancerAttributes | 
| ModifyRule | 
| ModifyTargetGroup | 
| ModifyTargetGroupAttributes | 
| RegisterInstancesWithLoadBalancer | 
| RegisterTargets | 
| RemoveTags | 
| SetLoadBalancerListenerSSLCertificate | 
| SetLoadBalancerPoliciesForBackendServer | 
| SetLoadBalancerPoliciesOfListener | 
| SetRulePriorities | 
| SetSecurityGroups | 
| SetSubnets | 

For more information, see [ Logging Elastic Load Balancing API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/ELB-API-Logs.html) in the *Elastic Load Balancing User Guide*\.

## Amazon Elasticsearch Service APIs<a name="view-cloudtrail-events-supported-apis-es"></a>


****  

|  | 
| --- |
| AddTags | 
| CreateElasticssearchDomain | 
| CreateElasticsearchServiceRole | 
| DeleteElasticsearchDomain | 
| RemoveTags | 
| UpdateElasticsearchDomainConfig | 

For information, see [Auditing Amazon Elasticsearch Service Domains with AWS CloudTrail](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html#es-managedomains-cloudtrailauditing) in the *Amazon Elasticsearch Service Developer Guide*\.

## AWS Elemental MediaStore APIs<a name="view-cloudtrail-events-supported-apis-ems"></a>


****  

|  | 
| --- |
| CreateContainer | 
| DeleteContainer | 
| DeleteContainerPolicy | 
| PutContainerPolicy | 

For more information, see [Logging AWS Elemental MediaStore API Calls with CloudTrail](http://docs.aws.amazon.com/mediastore/latest/ug/logging-using-cloudtrail.html) in the *AWS Elemental MediaStore User Guide*\.

## Amazon EMR APIs<a name="view-cloudtrail-events-supported-apis-elasticmapreduce"></a>


****  

|  | 
| --- |
| AddInstanceGroups | 
| AddJobFlowSteps | 
| AddTags | 
| CreateSecurityConfiguration | 
| DeleteSecurityConfiguration | 
| ModifyInstanceGroups | 
| RemoveTags | 
| RunJobFlow | 
| SetTerminationProtection | 
| SetVisibleToAllUsers | 
| TerminateJobFlows | 

For more information, see [ Logging Amazon EMR API Calls in AWS CloudTrail](http://docs.aws.amazon.com/emr/latest/ManagementGuide/logging_emr_api_calls.html) in the *Amazon EMR Management Guide*\.

## Amazon Elastic Transcoder APIs<a name="view-cloudtrail-events-supported-apis-elastictranscoder"></a>


****  

|  | 
| --- |
| CancelJob | 
| CreateJob | 
| CreateJobTemplate | 
| CreatePipeline | 
| CreatePreset | 
| CreateQueue | 
| DeleteJobTemplate | 
| DeletePipeline | 
| DeletePreset | 
| DeleteQueue | 
| TestRole | 
| UpdateJobTemplate | 
| UpgradeJobTemplate | 
| UpdatePipeline | 
| UpdatePipelineNotifications | 
| UpdatePipelineStatus | 
| UpdatePreset | 
| UpdateQueue | 

For more information, see [Logging Elastic Transcoder API Calls Using CloudTrail](http://docs.aws.amazon.com/elastictranscoder/latest/developerguide/logging_using_cloudtrail.html) in the *Amazon Elastic Transcoder Developer Guide*\.

## Amazon ElastiCache APIs<a name="view-cloudtrail-events-supported-apis-elasticache"></a>


****  

|  | 
| --- |
| AddTagsToResource | 
| AuthorizeCacheSecurityGroupIngress | 
| CopySnapshot | 
| CreateCacheCluster | 
| CreateCacheParameterGroup | 
| CreateCacheSecurityGroup | 
| CreateCacheSubnetGroup | 
| CreateReplicationGroup | 
| CreateSnapshot | 
| DeleteCacheCluster | 
| DeleteCacheParameterGroup | 
| DeleteCacheSecurityGroup | 
| DeleteCacheSubnetGroup | 
| DeleteReplicationGroup | 
| DeleteSnapshot | 
| ModifyCacheCluster | 
| ModifyCacheParameterGroup | 
| ModifyCacheSubnetGroup | 
| ModifyReplicationGroup | 
| PurchaseReservedCacheNodesOffering | 
| RebootCacheCluster | 
| RemoveTagsFromResource | 
| ResetCacheParameterGroup | 
| RevokeCacheSecurityGroupIngress | 

For more information, see [Logging Amazon ElastiCache API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Logging.html) in the *Amazon ElastiCache User Guide*\.

## AWS Firewall Manager API Calls<a name="view-cloudtrail-events-supported-apis-fms"></a>


****  

|  | 
| --- |
| DeletePolicy | 
| PutPolicy | 

For more information, see [Logging AWS WAF API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-using-cloudtrail.html) in the *AWS WAF Developer Guide*\.

## Amazon GameLift APIs<a name="view-cloudtrail-events-supported-apis-gamelift"></a>


****  

|  | 
| --- |
| AcceptMatch | 
| CreateAlias | 
| CreateBuild | 
| CreateFleet | 
| CreateGameSession | 
| CreateGameSessionQueue | 
| CreateMatchmakingConfiguration | 
| CreateMatchmakingRuleSet | 
| CreatePlayerSession | 
| CreatePlayerSessions | 
| CreateVpcPeeringAuthorization | 
| CreateVpcPeeringConnection | 
| DeleteAlias | 
| DeleteBuild | 
| DeleteFleet | 
| DeleteGameSessionQueue | 
| DeleteMatchmakingConfiguration | 
| DeleteVpcPeeringAuthorization | 
| DeleteVpcPeeringConnection | 
| StartGameSessionPlacement | 
| StartMatchBackfill | 
| StartMatchmaking | 
| StopGameSessionPlacement | 
| StopMatchmaking | 
| UpdateAlias | 
| UpdateBuild | 
| UpdateFleetAttributes | 
| UpdateFleetCapacity | 
| UpdateFleetPortSettings | 
| UpdateGameSession | 
| UpdateGameSessionQueue | 
| UpdateMatchmakingConfiguration | 
| ValidateMatchmakingRuleSet | 

For information about the GameLift calls logged by CloudTrail, see [Logging Amazon GameLift API Calls with AWS CloudTrail](http://docs.aws.amazon.com/gamelift/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon GameLift Developer Guide*\.

## Amazon Glacier APIs<a name="view-cloudtrail-events-supported-apis-glacier"></a>


****  

|  | 
| --- |
| AbortVaultLock | 
| AddTagsToVault | 
| CompleteVaultLock | 
| CreateVault | 
| DeleteVault | 
| DeleteVaultAccessPolicy | 
| DeleteVaultNotifications | 
| InitiateVaultLock | 
| RemoveTagsFromVault | 
| SetDataRetrievalPolicy | 
| SetVaultAccessPolicy | 
| SetVaultNotifications | 

For more information, see [Logging Amazon Glacier API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/amazonglacier/latest/dev/audit-logging.html) in the *Amazon Glacier Developer Guide*\.

## AWS Glue APIs<a name="view-cloudtrail-events-supported-apis-glue"></a>


****  

|  | 
| --- |
| BatchCreatePartition  | 
| BatchDeleteConnection  | 
| BatchDeletePartition  | 
| BatchDeleteTable  | 
| BatchStopJobRun  | 
| CreateClassifier  | 
| CreateConnection  | 
| CreateCrawler  | 
| CreateDatabase  | 
| CreateDevEndpoint  | 
| CreateJob  | 
| CreatePartition  | 
| CreateTable  | 
| CreateTrigger  | 
| CreateUserDefinedFunction  | 
| DeleteClassifier  | 
| DeleteConnection  | 
| DeleteCrawler  | 
| DeleteDatabase  | 
| DeleteDevEndpoint  | 
| DeleteJob  | 
| DeletePartition  | 
| DeleteTable  | 
| DeleteTrigger  | 
| DeleteUserDefinedFunction  | 
| ImportCatalogToGlue  | 
| ResetJobBookmark  | 
| StartCrawler  | 
| StartCrawlerSchedule  | 
| StartJobRun  | 
| StartTrigger  | 
| StopCrawler  | 
| StopCrawlerSchedule  | 
| StopTrigger  | 
| UpdateClassifier  | 
| UpdateConnection  | 
| UpdateCrawler  | 
| UpdateCrawlerSchedule  | 
| UpdateDatabase  | 
| UpdateDevEndpoint  | 
| UpdateJob  | 
| UpdatePartition  | 
| UpdateTable  | 
| UpdateTrigger  | 
| UpdateUserDefinedFunction  | 

For more information, see [Logging AWS Glue API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/glue/latest/dg/monitor-cloudtrail.html) in the *AWS Glue Developer Guide*\.

## Amazon GuardDuty APIs<a name="view-cloudtrail-events-supported-apis-guardduty"></a>


****  

|  | 
| --- |
| AcceptInvitation | 
| BatchFixMember | 
| BatchInviteAccount | 
| BatchRemoveMember | 
| BatchStageAccount | 
| BatchStartMonitoringMember | 
| BatchStopMonitoringMember | 
| CreateDetector | 
| CreateIPSet | 
| CreateThreatIntelSet | 
| DeclineInvitation | 
| DeleteDetector | 
| DeleteIPSet | 
| DeleteThreatIntelSet | 
| FixMember | 
| InviteAccount | 
| LeaveMaster | 
| RemoveMember | 
| StageAccount | 
| StartMonitoringMember | 
| StopMonitoringMember | 
| UpdateDetector | 
| UpdateIPSet | 
| UpdateThreatIntelSet | 

For more information, see the *Amazon GuardDuty User Guide*\.

## AWS Identity and Access Management APIs<a name="view-cloudtrail-events-supported-apis-iam"></a>


****  

|  | 
| --- |
| AddClientIDToOpenIDConnectProvider | 
| AddRoleToInstanceProfile | 
| AddUserToGroup | 
| AttachGroupPolicy | 
| AttachRolePolicy | 
| AttachUserPolicy | 
| ChangePassword | 
| CreateAccessKey | 
| CreateAccountAlias | 
| CreateGroup | 
| CreateInstanceProfile | 
| CreateLoginProfile | 
| CreateOpenIDConnectProvider | 
| CreatePolicy | 
| CreatePolicyVersion | 
| CreateRole | 
| CreateSAMLProvider | 
| CreateUser | 
| CreateVirtualMFADevice | 
| DeactivateMFADevice | 
| DeleteAccessKey | 
| DeleteAccountAlias | 
| DeleteAccountPasswordPolicy | 
| DeleteGroup | 
| DeleteGroupPolicy | 
| DeleteInstanceProfile | 
| DeleteLoginProfile | 
| DeleteOpenIDConnectProvider | 
| DeletePolicy | 
| DeletePolicyVersion | 
| DeleteRole | 
| DeleteRolePolicy | 
| DeleteSAMLProvider | 
| DeleteServerCertificate | 
| DeleteSigningCertificate | 
| DeleteSSHPublicKey | 
| DeleteUser | 
| DeleteUserPolicy | 
| DeleteVirtualMFADevice | 
| DetachGroupPolicy | 
| DetachRolePolicy | 
| DetachUserPolicy | 
| EnableMFADevice | 
| PutGroupPolicy | 
| PutRolePolicy | 
| PutUserPolicy | 
| RemoveClientIDFromOpenIDConnectProvider | 
| RemoveRoleFromInstanceProfile | 
| RemoveUserFromGroup | 
| ResyncMFADevice | 
| SetDefaultPolicyVersion | 
| UpdateAccessKey | 
| UpdateAccountPasswordPolicy | 
| UpdateAssumeRolePolicy | 
| UpdateGroup | 
| UpdateLoginProfile | 
| UpdateOpenIDConnectProviderThumbprint | 
| UpdateSAMLProvider | 
| UpdateServerCertificate | 
| UpdateSigningCertificate | 
| UpdateSSHPublicKey | 
| UpdateUser | 
| UploadServerCertificate | 
| UploadSigningCertificate | 
| UploadSSHPublicKey | 

For more information, see [Logging IAM Events with AWS CloudTrail](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html) in the *IAM User Guide*\.

## AWS Import/Export APIs<a name="view-cloudtrail-events-supported-apis-import-export"></a>


****  

|  | 
| --- |
| CancelJob | 
| CreateJob | 
| UpdateJob | 

For more information, see the [AWS Import/Export API Reference](http://docs.aws.amazon.com/AWSImportExport/latest/DG/api-reference.html)\.

## Amazon Inspector APIs<a name="view-cloudtrail-events-supported-apis-inspector"></a>


****  

|  | 
| --- |
| AddAttributesToFindings | 
| CreateAssessmentTarget | 
| CreateAssessmentTemplate | 
| CreateResourceGroup | 
| DeleteAssessmentRun | 
| DeleteAssessmentTarget | 
| DeleteAssessmentTemplate | 
| RegisterCrossAccountAccessRole | 
| RemoveAttributesFromFindings | 
| SetTagsForResource | 
| StartAssessmentRun | 
| StopAssessmentRun | 
| SubscribeToEvent | 
| UnsubscribeFromEvent | 
| UpdateAssessmentTarget | 

For more information, see [Logging Amazon Inspector API calls with AWS CloudTrail](http://docs.aws.amazon.com/inspector/latest/userguide/logging-using-cloudtrail.html) in the *Amazon Inspector User Guide*\.

## AWS IoT APIs<a name="view-cloudtrail-events-supported-apis-iot"></a>


****  

|  | 
| --- |
| AcceptCertificateTransfer | 
| AssociateTargetsWithJob | 
| AttachPrincipalPolicy | 
| AttachThingPrincipal | 
| CancelCertificateTransfer | 
| CreateCertificateFromCSR | 
| CreateJob | 
| CreateKeysAndCertificate | 
| CreatePolicy | 
| CreatePolicyVersion | 
| CreateThing | 
| CreateTopicRule | 
|  DeleteCACertificate  | 
| DeleteCertificate | 
| DeletePolicy | 
| DeletePolicyVersion | 
|  DeleteRegistrationCode   | 
| DeleteThing | 
| DeleteThingGroup | 
| DeleteThingType | 
| DeleteTopicRule | 
| DeleteV2LoggingLevel | 
| DeprecateThingType | 
| DetachPolicy | 
| DetachPrincipalPolicy | 
| DetachThingPrincipal | 
| DisableTopicRule | 
| EnableTopicRule | 
| ProvisionThing | 
|  RegisterCACertificate  | 
|  RegisterCertificate  | 
| RegisterThing | 
| RejectCertificateTransfer | 
| RemoveThingFromGroup | 
| ReplaceTopicRule | 
| SetDefaultPolicyVersion | 
| SetLoggingOptions | 
| SetV2LoggingLevel | 
| SetV2LoggingOptions | 
| StartThingRegistrationTask | 
| StopJob | 
| StopThingRegistrationTask | 
| TransferCertificate | 
| UpdateAuthorizer | 
|  UpdateCACertificate   | 
| UpdateCertificate | 
| UpdateCertificateTag | 
| UpdateEventConfigurations | 
| UpdateIndexingConfiguration | 
| UpdateRoleAlias | 
| UpdateThing | 
| UpdateThingGroup | 
| UpdateThingGroupsForThing | 

AWS IoT Analytics


|  | 
| --- |
| CreateChannel | 
| CreateDataset | 
| CreateDatasetContent | 
| CreateDatastore | 
| CreatePipeline | 
| DeleteChannel | 
| DeleteDataset | 
| DeleteDatasetContent | 
| DeleteDatastore | 
| DeletePipeline | 
| PutLoggingOptions | 
| RunPipelineActivity | 
| StartPipelineReprocessing | 
| UpdateChannel | 
| UpdateDataset | 
| UpdateDatastore | 
| UpdatePipeline | 

For more information, see [Logging AWS IoT API Calls with AWS CloudTrail](http://docs.aws.amazon.com/iot/latest/developerguide/monitoring_overview.html#iot-using-cloudtrail) in the *AWS IoT Developer Guide*\.

## Amazon Kinesis Data Firehose APIs<a name="view-cloudtrail-events-supported-apis-kinesis-firehose"></a>


****  

|  | 
| --- |
| CreateDeliveryStream | 
| DeleteDeliveryStream | 
| TagDeliveryStream | 
| UntagDeliveryStream | 
| UpdateDestination | 

For more information, see [Logging Amazon Kinesis Data Firehose API Calls with AWS CloudTrail](http://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudtrail.html) in the *Amazon Kinesis Data Firehose Developer Guide*\.

## Amazon Kinesis Data Streams APIs<a name="view-cloudtrail-events-supported-apis-kinesis"></a>


****  

|  | 
| --- |
| AddTagsToStream | 
| CreateStream | 
| DecreaseStreamRetentionPeriod | 
| DeleteStream | 
| DisableEnhancedMonitoring | 
| EnableEnhancedMonitoring | 
| IncreaseStreamRetentionPeriod | 
| MergeShards | 
| RemoveTagsFromStream | 
| SplitShard | 
| StartStreamEncryption | 
| StopStreamEncryption | 
| UpdateShardCount | 

For more information, see [Logging Amazon Kinesis Data Streams API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/kinesis/latest/dev/logging_using_cloudtrail.html) in the *Amazon Kinesis Developer Guide*\.

## AWS Lambda APIs<a name="view-cloudtrail-events-supported-apis-lambda"></a>


****  

|  | 
| --- |
| AddPermission20150331 | 
| AddPermission20150331v2 | 
| CreateAlias20150331 | 
| CreateEventSourceMapping20150331 | 
| CreateFunction20150331 | 
| DeleteAlias20150331 | 
| DeleteEventSourceMapping20150331 | 
| DeleteFunction | 
| DeleteFunction20150331 | 
| PublishVersion20150331 | 
| RemovePermission20150331 | 
| RemovePermission20150331v2 | 
| TagResource20170331 | 
| TagResource20170331v2 | 
| UntagResource20170331 | 
| UntagResource20170331v2 | 
| UpdateAlias20150331 | 
| UpdateEventSourceMapping20150331 | 
| UpdateFunctionCode | 
| UpdateFunctionCode20150331 | 
| UpdateFunctionCode20150331v2 | 
| UpdateFunctionConfiguration | 
| UpdateFunctionConfiguration20150331 | 
| UpdateFunctionConfiguration20150331v2 | 

<<<<<<< HEAD
For more information, see [Logging AWS Lambda API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/lambda/latest/dg/logging-using-cloudtrail.html) in the *AWS Lambda Developer Guide*\.
=======
For more information, see [Logging AWS Lambda API Calls Using AWS CloudTrail](https://docs.aws.amazon.com/lambda/latest/dg/logging-using-cloudtrail.html) in the *AWS Lambda Developer Guide*\.
>>>>>>> b83e3151f9f2aa169b73876bee166609f127d8f5

## Amazon Lex APIs<a name="view-cloudtrail-events-supported-apis-lex"></a>


****  

|  | 
| --- |
| CreateBotVersion | 
| CreateIntentVersion | 
| CreateSlotTypeVersion | 
| DeleteBot | 
| DeleteBotAlias | 
| DeleteBotChannelAssociation | 
| DeleteBotVersion | 
| DeleteIntent | 
| DeleteIntentVersion | 
| DeleteSlotType | 
| DeleteSlotTypeVersion | 
| DeleteUtterances | 
| PutBot | 
| PutBotAlias | 
| PutIntent | 
| PutSlotType" | 

For more information, see [Monitoring Amazon Lex API Calls With AWS CloudTrail Logs](http://docs.aws.amazon.com/lex/latest/dg/monitoring-aws-lex-cloudtrail.html) in the *Amazon Lex Developer Guide*\.

## AWS Amazon Lightsail APIs<a name="view-cloudtrail-events-supported-apis-lightsail"></a>


****  

|  | 
| --- |
| AllocateStaticIp | 
| AttachInstancesToLoadBalancer | 
| AttachLoadBalancerTlsCertificate | 
| CreateDiskFromSnapshot | 
| CreateDomain | 
| CreateDomainEntry | 
| CreateInstances | 
| CreateInstancesFromSnapshot | 
| CreateInstanceSnapshot | 
| CreateKeyPair | 
| CreateLoadBalancerTlsCertificate | 
| DeleteDisk | 
| DeleteDiskSnapshot | 
| DeleteInstanceSnapshot | 
| DeleteLoadBalancer | 
| DeleteLoadBalancerTlsCertificate | 
| DetachDisk | 
| DetachInstancesFromLoadBalancer | 
| DetachStaticIp | 
| ImportKeyPair | 
| PeerVpc | 
| RebootInstance | 
| StartInstance | 
| StopInstance | 
| UnpeerVpc | 
| UpdateDomainEntry | 
| UpdateLoadBalancerAttribute | 

For more information, see [Logging Lightsail API Calls with AWS CloudTrail](https://lightsail.aws.amazon.com/ls/docs/how-to/article/logging-lightsail-api-calls-using-aws-cloudtrail) in the *Amazon Lightsail Developer Guide*\.

## AWS Managed Services APIs<a name="view-cloudtrail-events-supported-apis-managed-services"></a>


****  

|  | 
| --- |
| ApproveRfc | 
| AssociateTechnicianWithRfc | 
| CancelRfc | 
| CreateRfc | 
| DisassociateTechnicianFromRfc | 
| PostCorrespondenceToRfc | 
| RejectRfc | 
| SubmitRfc | 
| UpdateRestrictedExecutionTimes | 
| UpdateRfcDescription | 
| UpdateRfcExecutionParameters | 
| UpdateRfcExpectedOutcome | 
| UpdateRfcImplementationPlan | 
| UpdateRfcRollbackPlan | 
| UpdateRfcSchedule | 
| UpdateRfcTitle | 
| UpdateRfcWorstCaseScenario | 

For more information, see [AWS Managed Services](https://aws.amazon.com/managed-services)\.

## AWS Marketplace Metering Service APIs<a name="view-cloudtrail-events-supported-apis-marketplace-metering"></a>


****  

|  | 
| --- |
| BatchMeterUsage | 

For more information, see the [AWS Marketplace Metering Service API Reference](http://docs.aws.amazon.com/marketplacemetering/latest/APIReference/Welcome.html)\.

## AWS Migration Hub APIs<a name="view-cloudtrail-events-supported-apis-migration-hub"></a>


****  

|  | 
| --- |
| AssociateCreatedArtifact | 
| AssociateResource | 
| CreateProgressUpdateStream | 
| DeleteProgressUpdateStream | 
| DisassociateCreatedArtifact | 
| DisassociateResource | 
| ImportMigrationTask | 
| NotifyMigrationTaskState | 
| PutResourceAttributes | 

For more information, see the [Logging AWS Migration Hub API Calls with AWS CloudTrail](http://docs.aws.amazon.com/migrationhub/latest/ug/logging-using-cloudtrail.html)\.

## AWS OpsWorks APIs<a name="view-cloudtrail-events-supported-apis-opsworks"></a>

**AWS OpsWorks**


****  

|  | 
| --- |
| AssignInstance | 
| AssignVolume | 
| AssociateElasticIp | 
| AttachElasticLoadBalancer | 
| CloneStack | 
| CreateApp | 
| CreateDeployment | 
| CreateInstance | 
| CreateLayer | 
| CreateStack | 
| CreateUserProfile | 
| DeleteApp | 
| DeleteInstance | 
| DeleteLayer | 
| DeleteStack | 
| DeleteUserProfile | 
| DeregisterElasticIp | 
| DeregisterEcsCluster | 
| DeregisterInstance | 
| DeregisterRdsDbInstance | 
| DeregisterVolume | 
| DetachElasticLoadBalancer | 
| DisassociateElasticIp | 
| GrantAccess | 
| RebootInstance | 
| RegisterEcsCluster | 
| RegisterElasticIp | 
| RegisterInstance | 
| RegisterRdsDbInstance | 
| RegisterVolume | 
| SetLoadBasedAutoScaling | 
| SetPermission | 
| SetTimeBasedAutoScaling | 
| StartInstance | 
| StartStack | 
| StopInstance | 
| StopStack | 
| UnassignInstance | 
| UnassignVolume | 
| UpdateApp | 
| UpdateElasticIp | 
| UpdateInstance | 
| UpdateLayer | 
| UpdateMyUserProfile | 
| UpdateRdsDbInstance | 
| UpdateStack | 
| UpdateUserProfile | 
| UpdateVolume | 

**AWS OpsWorks for configuration management**


****  

|  | 
| --- |
| CreateBackup | 
| CreateServer | 
| DeleteBackup | 
| DeleteServer | 
| RestoreServer | 
| StartMaintenance | 
| UpdateServer | 
| UpdateServerEngineAttributes | 

For more information, see [Logging AWS OpsWorks API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/opsworks/latest/userguide/monitoring-cloudtrail.html) in the *AWS OpsWorks User Guide*\.

## AWS Organizations Events<a name="view-cloudtrail-events-supported-apis-organizations"></a>


****  

|  | 
| --- |
| AcceptHandshake | 
| AttachPolicy | 
| CancelHandshake | 
| CreateAccount | 
| CreateOrganization | 
| CreateOrganizationalUnit | 
| CreatePolicy | 
| DeclineHandshake | 
| DeleteOrganization | 
| DeleteOrganizationalUnit | 
| DeletePolicy | 
| DetachPolicy | 
| DisableAWSServiceAccess | 
| DisablePolicyType | 
| EnableAllFeatures | 
| EnableAWSServiceAccess | 
| EnablePolicyType | 
| InviteAccountToOrganization | 
| LeaveOrganization | 
| MoveAccount | 
| RemoveAccountFromOrganization | 
| UpdateOrganizationalUnit | 
| UpdatePolicy | 

For more information, see [Logging AWS Organizations API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/organizations/latest/userguide/orgs_monitoring.html#orgs_cloudtrail-integration) in the *AWS Organizations User Guide*\.

## AWS Personal Health Dashboard APIs<a name="view-cloudtrail-events-supported-apis-phd"></a>


****  

|  | 
| --- |
| DescribeEvents | 
| DescribeEventAggregates | 
| DescribeEventtypes | 

For more information, see [Logging AWS Health API Calls with AWS CloudTrail](http://docs.aws.amazon.com/health/latest/ug/logging-using-cloudtrail.html) in the *AWS Health User Guide*\.

## Amazon Pinpoint APIs<a name="view-cloudtrail-events-supported-apis-pin"></a>


****  

|  | 
| --- |
| CreateApp | 
| CreateCampaign | 
| CreateExportJob | 
| CreateImportJob | 
| CreateSegment | 
| DeleteAdmChannel | 
| DeleteApnsChannel | 
| DeleteApnsSandboxChannel | 
| DeleteApp | 
| DeleteBaiduChannel | 
| DeleteCampaign | 
| DeleteEmailChannel | 
| DeleteEventStream | 
| DeleteGcmChannel | 
| DeleteSegment | 
| DeleteSmsChannel | 
| PutEventStream | 
| UpdateAdmChannel | 
| UpdateApnsChannel | 
| UpdateApnsSandboxChannel | 
| UpdateApplicationSettings | 
| UpdateBaiduChannel | 
| UpdateCampaign | 
| UpdateEmailChannel | 
| UpdateGcmChannel | 
| UpdateSegment | 

For more information, see [Logging Amazon Pinpoint API Calls with AWS CloudTrail](http://docs.aws.amazon.com/pinpoint/latest/developerguide/logging-using-cloudtrail.html) in the *Amazon Pinpoint Developer Guide*\.

## Amazon Polly APIs<a name="view-cloudtrail-events-supported-apis-pol"></a>


****  

|  | 
| --- |
| PutLexicon | 
| SynthesizeSpeech | 

For more information, see [Logging Amazon Polly API Calls with AWS CloudTrail](http://docs.aws.amazon.com/polly/latest/dg/logging-using-cloudtrail.html) in the *Amazon Polly Developer Guide*\.

## Amazon QuickSight Events<a name="view-cloudtrail-events-supported-apis-quicksight"></a>


****  

|  | 
| --- |
| BatchCreateUser | 
| BatchResendUserInvite | 
| CreateAccount | 
| CreateAnalysis | 
| CreateDashboard | 
| CreateDataSet | 
| CreateDataSource | 
| CreatePermission | 
| CreateSpiceCapacity | 
| CreateSubscription | 
| DeleteAccount | 
| DeleteAnalysis | 
| DeleteDashboard | 
| DeleteDataSet | 
| DeleteDataSource | 
| DeleteSpiceCapacity | 
| DeleteSubscription | 
| DeleteUser | 
| ImportS3ManifestFile | 
| UpdateAccountSettings | 
| UpdateAnalysis | 
| UpdateAnalysisAccess | 
| UpdateDashboard | 
| UpdateDashboardAccess | 
| UpdateDataSet | 
| UpdateDataSource | 
| UpdateGroups | 
| UpdateSubscription | 

For more information, see [Logging Operations with CloudTrail](http://docs.aws.amazon.com/quicksight/latest/user/logging-using-cloudtrail.html) in the *Amazon QuickSight User Guide*\.

## Amazon Redshift APIs<a name="view-cloudtrail-events-supported-apis-redshift"></a>


****  

|  | 
| --- |
| AuthorizeClusterSecurityGroupIngress | 
| AuthorizeSnapshotAccess | 
| CopyClusterSnapshot | 
| CreateCluster | 
| CreateClusterParameterGroup | 
| CreateClusterSecurityGroup | 
| CreateClusterSnapshot | 
| CreateClusterSubnetGroup | 
| CreateEventSubscription | 
| CreateHsmClientCertificate | 
| CreateHsmConfiguration | 
| CreateTags | 
| DeleteCluster | 
| DeleteClusterParameterGroup | 
| DeleteClusterSecurityGroup | 
| DeleteClusterSnapshot | 
| DeleteClusterSubnetGroup | 
| DeleteEventSubscription | 
| DeleteHsmClientCertificate | 
| DeleteHsmConfiguration | 
| DeleteTags | 
| DisableLogging | 
| DisableSnapshotCopy | 
| EnableLogging | 
| EnableSnapshotCopy | 
| ModifyCluster | 
| ModifyClusterIamRoles | 
| ModifyClusterParameterGroup | 
| ModifyClusterSubnetGroup | 
| ModifyEventSubscription | 
| ModifySnapshotCopyRetentionPeriod | 
| PurchaseReservedNodeOffering | 
| RebootCluster | 
| ResetClusterParameterGroup | 
| RestoreFromClusterSnapshot | 
| RestoreTableFromClusterSnapshot | 
| RevokeClusterSecurityGroupIngress | 
| RevokeSnapshotAccess | 
| RotateEncryptionKey | 

For more information, see [Using AWS CloudTrail for Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html#rs-db-auditing-cloud-trail) in the *Amazon Redshift Cluster Management Guide*\.

## Amazon Rekognition APIs<a name="view-cloudtrail-events-supported-apis-rekognition"></a>


****  

|  | 
| --- |
| CreateCollection | 
| CreateStreamProcessor | 
| DeleteCollection | 
| DeleteStreamProcessor | 

For more information, see the [Logging Amazon Rekognition API Calls with AWS CloudTrail](http://docs.aws.amazon.com/rekognition/latest/dg/logging-using-cloudtrail.html)\.

## Amazon Relational Database Service APIs<a name="view-cloudtrail-events-supported-apis-rds"></a>


****  

|  | 
| --- |
| AddRoleToDbCluster | 
| AddSourceIdentifierToSubscription | 
| AddTagsToResource | 
| ApplyPendingMaintenanceAction | 
| AuthorizeDBSecurityGroupIngress | 
| CopyDbClusterParameterGroup | 
| CopyDBParameterGroup | 
| CopyDBSnapshot | 
| CopyOptionGroup | 
| CreateDBCluster | 
| CreateDBClusterParameterGroup | 
| CreateDBClusterSnapshot | 
| CreateDBInstance | 
| CreateDBInstanceReadReplica | 
| CreateDBParameterGroup | 
| CreateDBSecurityGroup | 
| CreateDBSnapshot | 
| CreateDBSubnetGroup | 
| CreateEventSubscription | 
| CreateOptionGroup | 
| DeleteDBCluster | 
| DeleteDBClusterParameterGroup | 
| DeleteDBClusterSnapshot | 
| DeleteDBInstance | 
| DeleteDBParameterGroup | 
| DeleteDBSecurityGroup | 
| DeleteDBSnapshot | 
| DeleteDBSubnetGroup | 
| DeleteEventSubscription | 
| DeleteOptionGroup | 
| FailoverDBCluster | 
| ModifyDBCluster | 
| ModifyDBClusterParameterGroup | 
| ModifyDbClusterSnapshotAttribute | 
| ModifyDBInstance | 
| ModifyDBParameterGroup | 
| ModifyDBSnapshotAttribute | 
| ModifyDBSubnetGroup | 
| ModifyEventSubscription | 
| ModifyOptionGroup | 
| PromoteReadReplica | 
| PromoteReadReplicaDbCluster | 
| PurchaseReservedDBInstancesOffering | 
| RebootDBInstance | 
| RemoveRoleFromDbCluster | 
| RemoveSourceIdentifierFromSubscription | 
| RemoveTagsFromResource | 
| ResetDBClusterParameterGroup | 
| ResetDBParameterGroup | 
| RestoreDbClusterFromS3 | 
| RestoreDBClusterFromSnapshot | 
| RestoreDBClusterToPointInTime | 
| RestoreDBInstanceFromDBSnapshot | 
| RestoreDbInstanceFromS3 | 
| RestoreDBInstanceToPointInTime | 
| RevokeDBSecurityGroupIngress | 
| StopDBInstance | 

For more information, see [Logging Amazon RDS API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Auditing.html) in the *Amazon Relational Database Service User Guide*\.

## Amazon Route 53 APIs<a name="view-cloudtrail-events-supported-apis-route53"></a>

**Amazon Route 53**


****  

|  | 
| --- |
| AssociateVPCWithHostedZone | 
| ChangeTagsForResource | 
| ChangeResourceRecordSets | 
| CreateHealthCheck | 
| CreateHostedZone | 
| CreateReusableDelegationSet | 
| CreateTrafficPolicy | 
| CreateTrafficPolicyInstance | 
| CreateTrafficPolicyVersion | 
| DeleteHealthCheck | 
| DeleteHostedZone | 
| DeleteReusableDelegationSet | 
| DeleteTrafficPolicy | 
| DeleteTrafficPolicyInstance | 
| DisassociateVPCFromHostedZone | 
| UpdateHealthCheck | 
| UpdateHostedZoneComment | 
| UpdateTrafficPolicyComment | 
| UpdateTrafficPolicyInstance | 

**Amazon Route 53 Auto Naming**


****  

|  | 
| --- |
| CreatePrivateDnsNamespace | 
| CreatePublicDnsNamespace | 
| CreateService | 
| DeleteNamespace | 
| DeleteService | 
| DeregisterInstance | 
| RegisterInstance | 
| UpdateInstanceCustomHealthStatus | 
| UpdateService | 

**Amazon Route 53 Domains**


****  

|  | 
| --- |
| AddDnssec | 
| BatchTransferDomain | 
| ChangeDomainOwner | 
| DeleteAllTagsForDomain | 
| DeleteDomain | 
| DeleteTagsForDomain | 
| DisableAutoRenew | 
| EnableAutoRenew | 
| GetAuthCode | 
| LockDomain | 
| PostTagsForDomain | 
| PushDomain | 
| RegisterDomain | 
| RemoveDnssec | 
| RenewDomain | 
| SaveContactTemplates | 
| TransferDomain | 
| UnlockDomain | 
| UpdateDomainContact | 
| UpdateNameservers | 
| UpdatePrivacyProtection | 

For more information, see [Using AWS CloudTrail to Capture Requests Sent to the Route 53 API](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/logging-using-cloudtrail.html) in the *Amazon Route 53 Developer Guide*\.

## Amazon SageMaker APIs<a name="view-cloudtrail-events-supported-apis-sm"></a>


****  

|  | 
| --- |
| AddTags | 
| CreateEndpoint | 
| CreateEndpointConfig | 
| CreateHyperParameterTuningJob | 
| CreateModel | 
| CreateNotebookInstance | 
| CreateNotebookInstanceLifecycleConfig | 
| CreatePresignedNotebookInstanceUrl | 
| CreateTrainingJob | 
| DeleteEndpoint | 
| DeleteEndpointConfig | 
| DeleteModel | 
| DeleteNotebookInstance | 
| DeleteNotebookInstanceLifecycleConfig | 
| DeleteTags | 
| StartNotebookInstance | 
| StopHyperParameterTuningJob | 
| StopNotebookInstance | 
| StopTrainingJob | 
| UpdateEndpoint | 
| UpdateEndpointWeightsAndCapacities | 
| UpdateNotebookInstance | 
| UpdateNotebookInstanceLifecycleConfig | 

For more information, see [Logging Amazon SageMaker API Calls with AWS CloudTrail](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-using-cloudtrail.html) in the *Amazon SageMaker Developer Guide*\.

## AWS Secrets Manager APIs<a name="view-cloudtrail-events-supported-apis-asm"></a>


****  

|  | 
| --- |
| CancelRotateSecret | 
| CancelSecretVersionDelete | 
| CreateSecret | 
| DeleteSecret | 
| EndSecretVersionDelete | 
| PutSecretValue | 
| RestoreSecret | 
| RotateSecret | 
| RotationFailed | 
| RotationStarted | 
| RotationSucceeded | 
| StartSecretVersionDelete | 
| TagResource | 
| UntagResource | 
| UpdateSecret | 
| UpdateSecretVersionStage | 

For more information, see [Monitor the Use of Your AWS Secrets Manager Secrets](http://docs.aws.amazon.com/secretsmanager/latest/userguide/monitoring.html#monitoring_cloudtrail) in the *AWS Secrets Manager User Guide*\.

## AWS Server Migration Service APIs<a name="view-cloudtrail-events-supported-apis-sms"></a>


****  

|  | 
| --- |
| CreateReplicationJob | 
| DeleteReplicationJob | 
| DeleteServerCatalog | 
| DisassociateConnector | 
| ImportServerCatalog | 
| SendMessage | 
| StartReplicationRun | 
| UpdateReplicationJob | 

For more information, see the [AWS SMS API Reference](http://docs.aws.amazon.com/ServerMigration/latest/APIReference/)\.

## AWS Service Catalog APIs<a name="view-cloudtrail-events-supported-apis-service-catalog"></a>


****  

|  | 
| --- |
| AcceptPortfolioShare | 
| AssociatePrincipalWithPortfolio | 
| AssociateProductWithPortfolio | 
| CreateConstraint | 
| CreatePortfolio | 
| CreatePortfolioShare | 
| CreateProduct | 
| CreateProvisioningArtifact | 
| DeleteConstraint | 
| DeletePortfolio | 
| DeletePortfolioShare | 
| DeleteProduct | 
| DeleteProvisioningArtifact | 
| DisassociatePrincipalFromPortfolio | 
| DisassociateProductFromPortfolio | 
| ProvisionProduct | 
| RejectPortfolioShare | 
| TerminateProvisionedProduct | 
| UpdateConstraint | 
| UpdatePortfolio | 
| UpdateProduct | 
| UpdateProvisioningArtifact | 
| UpdateProvisionedProduct | 

For information, see [Logging AWS Service Catalog API Calls with AWS CloudTrail](http://docs.aws.amazon.com/servicecatalog/latest/dg/logging-using-cloudtrail.html) in the *AWS Service Catalog Developer Guide*\.

## AWS Shield APIs<a name="view-cloudtrail-events-supported-apis-shield"></a>


****  

|  | 
| --- |
| CreateProtection | 
| CreateSubscription | 
| DeleteProtection | 
| DeleteSubscription | 

For information, see [Logging Shield Advanced API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-shield-using-cloudtrail.html) in the *AWS WAF Developer Guide*\.

## Amazon Simple Storage Service \(S3\) Bucket Level APIs<a name="view-cloudtrail-events-supported-apis-s3"></a>


****  

|  | 
| --- |
| CreateBucket | 
| DeleteBucket | 
| DeleteBucketCors | 
| DeleteBucketEncryption | 
| DeleteBucketLifecycle | 
| DeleteBucketPolicy | 
| DeleteBucketReplication | 
| DeleteBucketTagging | 
| DeleteBucketWebsite | 
| PutBucketAcl | 
| PutBucketCors | 
| PutBucketEncryption | 
| PutBucketLifecycle | 
| PutBucketLogging | 
| PutBucketNotification | 
| PutBucketPolicy | 
| PutBucketReplication | 
| PutBucketRequestPayment | 
| PutBucketTagging | 
| PutBucketVersioning | 
| PutBucketWebsite | 

Event history doesn't support Amazon S3 object\-level API operations such as `DeleteObject`, `PostObject`, and `PutObject`\. You can view events for these operations in the log files delivered to your S3 bucket\. For more information, see [Logging Amazon S3 API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonS3/latest/dev/cloudtrail-logging.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Amazon Simple Workflow Service APIs<a name="view-cloudtrail-events-supported-apis-swf"></a>


****  

|  | 
| --- |
| DeprecateActivityType | 
| DeprecateDomain | 
| DeprecateWorkflowType | 
| RegisterActivityType | 
| RegisterDomain | 
| RegisterWorkflowType | 

For more information, see [Logging Amazon Simple Workflow Service API Calls with AWS CloudTrail](http://docs.aws.amazon.com/amazonswf/latest/developerguide/ct-logging.html) in the *Amazon Simple Workflow Service Developer Guide*\.

## AWS Single Sign\-On APIs<a name="view-cloudtrail-events-supported-apis-sso"></a>


****  

|  | 
| --- |
| AssociateDirectory | 
| AssociateProfile | 
| CreateApplicationInstance | 
| CreateApplicationInstanceCertificate | 
| CreatePermissionSet | 
| CreateProfile | 
| DeleteApplicationInstance | 
| DeleteApplicationInstanceCertificate | 
| DeletePermissionSet | 
| DeletePermissionsPolicy | 
| DeleteProfile | 
| DisassociateDirectory | 
| DisassociateProfile | 
| ImportApplicationInstanceServiceProviderMetadata | 
| PutPermissionsPolicy | 
| StartSSO | 
| UpdateApplicationInstanceActiveCertificate | 
| UpdateApplicationInstanceDisplayData | 
| UpdateApplicationInstanceResponseConfiguration | 
| UpdateApplicationInstanceResponseSchemaConfiguration | 
| UpdateApplicationInstanceSecurityConfiguration | 
| UpdateApplicationInstanceServiceProviderConfiguration | 
| UpdateApplicationInstanceStatus | 
| UpdateDirectoryAssociation | 
| UpdateProfile | 

For more information, see [Logging AWS SSO API Calls with AWS CloudTrail](http://docs.aws.amazon.com/singlesignon/latest/userguide/logging-using-cloudtrail.html) in the *AWS Single Sign\-On User Guide*\.

## AWS Step Functions APIs<a name="view-cloudtrail-events-supported-apis-step-functions"></a>


****  

|  | 
| --- |
| CreateActivity | 
| CreateStateMachine | 
| DeleteActivity | 
| DeleteStateMachine | 
| StartExecution | 
| StopExecution | 

For more information, see [Logging AWS Step Functions API Calls with AWS CloudTrail](http://docs.aws.amazon.com/step-functions/latest/dg/cloud-trail.html) in the *AWS Step Functions Developer Guide*\.

## AWS Storage Gateway APIs<a name="view-cloudtrail-events-supported-apis-storage-gateway"></a>


****  

|  | 
| --- |
| ActivateGateway | 
| AddCache | 
| AddUploadBuffer | 
| AddWorkingStorage | 
| CancelArchival | 
| CancelRetrieval | 
| CreateCachediSCSIVolume | 
| CreateNfsFileShare | 
| CreateSnapshot | 
| CreateSnapshotFromVolumeRecoveryPoint | 
| CreateStorediSCSIVolume | 
| CreateTapes | 
| DeleteBandwidthRateLimit | 
| DeleteChapCredentials | 
| DeleteFileShare | 
| DeleteGateway | 
| DeleteSnapshotSchedule | 
| DeleteTape | 
| DeleteTapeArchive | 
| DeleteVolume | 
| DisableGateway | 
| RefreshCache | 
| ShutdownGateway | 
| StartGateway | 
| UpdateBandwidthRateLimit | 
| UpdateChapCredentials | 
| UpdateGatewayInformation | 
| UpdateGatewaySoftwareNow | 
| UpdateMaintenanceStartTime | 
| UpdateNfsFileShare | 
| UpdateSnapshotSchedule | 
| UpdateVTLDeviceType | 

For more information, see [Logging AWS Storage Gateway API Calls by Using AWS CloudTrail](http://docs.aws.amazon.com/storagegateway/latest/userguide/logging-using-cloudtrail.html) in the *AWS Storage Gateway User Guide*\.

## AWS Support APIs<a name="view-cloudtrail-events-supported-apis-aws-support"></a>


****  

|  | 
| --- |
| AddAttachmentsToSet | 
| AddCommunicationToCase | 
| CreateCase | 
| ResolveCase | 

For more information, see [Logging AWS Support API Calls with AWS CloudTrail](http://docs.aws.amazon.com/awssupport/latest/user/logging-using-cloudtrail.html) in the *AWS Support User Guide*\.

## AWS WAF APIs<a name="view-cloudtrail-events-supported-apis-waf"></a>


****  

|  | 
| --- |
| CreateByteMatchSet | 
| CreateIPSet | 
| CreateRule | 
| CreateSizeConstraintSet | 
| CreateSqlInjectionMatchSet | 
| CreateWebACL | 
| CreateXssMatchSet | 
| DeleteByteMatchSet | 
| DeleteIPSet | 
| DeleteRule | 
| DeleteSizeConstraintSet | 
| DeleteSqlInjectionMatchSet | 
| DeleteWebACL | 
| DeleteXssMatchSet | 
| UpdateByteMatchSet | 
| UpdateIPSet | 
| UpdateRule | 
| UpdateSizeConstraintSet | 
| UpdateSqlInjectionMatchSet | 
| UpdateWebACL | 
| UpdateXssMatchSet | 

For more information, see [Logging AWS WAF API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-using-cloudtrail.html) in the *AWS WAF Developer Guide*\.

## Amazon WorkMail APIs<a name="view-cloudtrail-events-supported-apis-wm"></a>


****  

|  | 
| --- |
| AddMembersToGroup | 
| CreateAlias | 
| CreateGroup | 
| CreateMailUser | 
| CreateResource | 
| CreateUser | 
| DeleteFreeBusyConfiguration | 
| DeleteGroup | 
| DeleteMailDomain | 
| DeleteMailFlowRule | 
| DeleteResource | 
| DeleteResources | 
| DeregisterFromWorkMail | 
| DisableInteroperabilityMode | 
| DisableMailGroups | 
| DisableMailUsers | 
| DisassociateDelegateFromResource | 
| DisassociateMemberFromGroup | 
| EnableMailDomain | 
| EnableMailUsers | 
| EvaluateMailFlowRules | 
| PutMailboxPermissions | 
| RemoveMembersFromGroup | 
| ResetPassword | 
| SetJournalingRules | 
| SetMobilePolicyDetails | 
| UpdateMailFlowRule | 
| UpdatePrimaryEmailAddress | 
| UpdateResource | 
| UpdateUser | 

For more information, see [Logging Amazon WorkMail API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/workmail/latest/adminguide/logging-using-cloudtrail.html) in the *Amazon WorkMail Administrator Guide*\.

## AWS X\-Ray APIs<a name="console-view-cloudtrail-events-supported-apis-xray"></a>


****  

|  | 
| --- |
| PutEncryptionConfig | 

For more information, see [Logging AWS X\-Ray API Calls With CloudTrail](http://docs.aws.amazon.com/xray/latest/devguide/xray-api-cloudtrail.html) in the *AWS X\-Ray Developer Guide*\.

## AWS Billing Events<a name="console-billing-events"></a>

You can look up non\-API events in your account, such as changes to your billing information for your AWS account\.


****  

|  | 
| --- |
| AcceptFxPaymentCurrencyTermsAndConditions | 
| CloseAccount | 
| SetAccountContractMetadata | 
| SetAdditionalContacts | 
| SetContactAddress | 
| SetFxPaymentCurrency | 
| SetIAMAccessPreference | 
| SetSecurityQuestions | 

For more information, see [AWS Billing and Cost Management User Guide](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/)\.

## AWS Console Sign\-in Events<a name="console-sign-in-events"></a>

You can also look up non\-API events in your account, such as signing in the AWS console or switching a role\.


****  

|  | 
| --- |
| ConsoleLogin | 
| ExitRole | 
| RenewRole | 
| SwitchRole | 

For more information, see [AWS Console Sign\-in Events](cloudtrail-event-reference-aws-console-sign-in-events.md)\.
