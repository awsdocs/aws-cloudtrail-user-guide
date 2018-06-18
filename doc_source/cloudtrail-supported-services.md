# CloudTrail Supported Services<a name="cloudtrail-supported-services"></a>

CloudTrail supports the following services\. 

**Note**  
To see the list of supported regions for each service, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.


+ [Additional Software & Services](#additional-software-and-services)
+ [Analytics](#cloudtrail-supported-services-analytics)
+ [Application Integration](#cloudtrail-supported-services-application-services)
+ [Business Productivity](#cloudtrail-supported-services-business-productivity)
+ [Compute](#cloudtrail-supported-services-compute)
+ [Customer Engagement](#cloudtrail-supported-services-customer-engagement)
+ [Database](#cloudtrail-supported-services-database)
+ [Desktop & App Streaming](#cloudtrail-supported-services-desktop-and-app-streaming)
+ [Developer Tools](#cloudtrail-supported-services-developer-tools)
+ [Game Development](#cloudtrail-supported-services-game-development)
+ [Internet of Things](#cloud-trail-supported-services-internet-of-things)
+ [Machine Learning](#cloudtrail-supported-services-artificial-intelligence)
+ [Management Tools](#cloudtrail-supported-services-management-tools)
+ [Media Services](#cloudtrail-supported-services-media-services)
+ [Migration](#cloudtrail-supported-services-migration)
+ [Mobile Services](#cloudtrail-supported-services-mobile-services)
+ [Networking & Content Delivery](#cloudtrail-supported-services-networking)
+ [Security, Identity & Compliance](#cloudtrail-supported-services-security-and-identity)
+ [Storage](#cloudtrail-supported-services-storage-and-content-delivery)
+ [Support](#cloud-trail-supported-services-aws-support)
+ [Services That Support Logging Data Events](#cloud-trail-supported-services-data-events)
+ [CloudTrail Integration Topics by AWS Service](cloudtrail-aws-service-specific-topics.md)
+ [CloudTrail Unsupported Services](cloudtrail-unsupported-aws-services.md)

## Additional Software & Services<a name="additional-software-and-services"></a>

 **AWS Marketplace**  
AWS Marketplace is an online store where you can buy or sell software that runs on AWS\. As a subscriber, you can find, buy, and quickly deploy software that runs on AWS\. As a seller, you can manage the sales channel for products you sell\. For more information, see [AWS Marketplace](https://aws.amazon.com/marketplace?b_k=291)\. CloudTrail supports logging only the `BatchMeterUsage` action\. For information, see the [AWS Marketplace Metering Service API Reference](http://docs.aws.amazon.com/marketplacemetering/latest/APIReference/Welcome.html)\.   
Support began 05/02/2017

## Analytics<a name="cloudtrail-supported-services-analytics"></a>

**Amazon Athena**  
Amazon Athena is an interactive query service that makes it easy to analyze data directly in Amazon Simple Storage Service using standard SQL\. With a few actions in the AWS Management Console, you can point Athena at your data stored in Amazon S3 and begin using standard SQL to run ad\-hoc queries and get results in seconds\. For more information, see the [Amazon Athena User Guide](http://docs.aws.amazon.com/athena/latest/ug/)\.  
For more information about the Athena calls logged by CloudTrail, see [Logging Amazon Athena API Calls with AWS CloudTrail](http://docs.aws.amazon.com/athena/latest/ug/monitor-with-cloudtrail.html)\.  
Support began 05/19/2017

 **Amazon CloudSearch**   
Amazon CloudSearch is a fully\-managed service in the cloud that makes it easy to set up, manage, and scale a search solution for your website\. Amazon CloudSearch enables you to search large collections of data such as web pages, document files, forum posts, or product information\. For more information, see the [Amazon CloudSearch Developer Guide](http://docs.aws.amazon.com/cloudsearch/latest/developerguide/)\. For information about the Amazon CloudSearch calls logged by CloudTrail, see [Logging Amazon CloudSearch Configuration Service Calls Using AWS CloudTrail](http://docs.aws.amazon.com/cloudsearch/latest/developerguide/logging-config-api-calls.html)\.  
Support began 10/16/2014

 **Amazon Elasticsearch Service**   
Amazon Elasticsearch Service is a managed service that makes it easy to deploy, operate, and scale Amazon ES in the AWS cloud\. For more information, see the [Amazon Elasticsearch Service Developer Guide](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/)\. For information about the Amazon ES API calls logged by CloudTrail, see [Auditing Amazon Elasticsearch Service Domains with AWS CloudTrail](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html#es-managedomains-cloudtrailauditing)\.  
Support began 10/01/2015

 **Amazon EMR**   
Amazon EMR is a web service that makes it easy to process large amounts of data efficiently\. Amazon EMR uses Hadoop processing combined with several services from AWS to perform such tasks as web indexing, data mining, log file analysis, machine learning, scientific simulation, and data warehousing\. For more information, see the [Amazon EMR Developer Guide](http://docs.aws.amazon.com/emr/latest/DeveloperGuide/)\. For information about the Amazon EMR calls logged by CloudTrail, see [Logging Amazon EMR API Calls in AWS CloudTrail](http://docs.aws.amazon.com/emr/latest/ManagementGuide/logging_emr_api_calls.html)\.  
Support began 04/04/2014

 **AWS Data Pipeline**   
AWS Data Pipeline is a web service that you can use to automate the movement and transformation of data through data\-driven workflows\. For more information, see the [AWS Data Pipeline Developer Guide](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/)\. For information about the AWS Data Pipeline calls logged by CloudTrail, see [Logging AWS Data Pipeline API Calls by Using AWS CloudTrail](http://docs.aws.amazon.com/datapipeline/latest/DeveloperGuide/dp-cloudtrail-logging.html)\.  
Support began 12/02/2014

 **AWS Glue**   
AWS Glue is a fully managed ETL \(extract, transform, and load\) service that makes it simple and cost\-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores\.  
 For more information, see the [AWS Glue Developer Guide](http://docs.aws.amazon.com/glue/latest/dg/)\. For information about the AWS Glue calls logged by CloudTrail, see [Logging AWS Glue Operations Using AWS CloudTrail](http://docs.aws.amazon.com/glue/latest/dg/monitor-cloudtrail.html)\.  
Support began 11/07/2017

 **Amazon Kinesis Data Firehose**   
Amazon Kinesis Data Firehose is a fully managed service for delivering real\-time streaming data to destinations such as Amazon S3 and Amazon Redshift\. Kinesis Data Firehose is part of the Kinesis streaming data family, along with Amazon Kinesis Data Streams\. For more information, see the [Amazon Kinesis Data Firehose Developer Guide](http://docs.aws.amazon.com/firehose/latest/dev/)\. For information about the Kinesis Data Firehose calls logged by CloudTrail, see [Monitoring Amazon Kinesis Data Firehose API Calls with AWS CloudTrail](http://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudtrail.html)\.  
Support began 03/17/2016

 **Amazon Kinesis Data Streams**   
Amazon Kinesis Data Streams is a managed service that scales elastically for real\-time processing of streaming big data\. The service takes in large streams of data records that can then be consumed in real time by multiple data\-processing applications that can be run on Amazon EC2 instances\. For more information, see the [Amazon Kinesis Data Streams Developer Guide](http://docs.aws.amazon.com/streams/latest/dev/)\. For information about the Kinesis Data Streams calls logged by CloudTrail, see [Logging Amazon Kinesis Data Streams API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/kinesis/latest/dev/logging_using_cloudtrail.html)\.  
Support began 04/25/2014

 **Amazon QuickSight**   
Amazon QuickSight is a business analytics service you can use to build visualizations, perform ad hoc analysis, and quickly get business insights from your data\. Amazon QuickSight seamlessly discovers AWS data sources, enables organizations to scale to hundreds of thousands of users, and delivers fast and responsive query performance by using a robust in\-memory engine \(SPICE\)\.   
For more information, see the [Amazon QuickSight User Guide](http://docs.aws.amazon.com/quicksight/latest/user/)\. For information about the Amazon QuickSight operations logged by CloudTrail, see [Logging Operations with CloudTrail](http://docs.aws.amazon.com/quicksight/latest/user/logging-using-cloudtrail.html)\.  
Support began 04/28/2017

## Application Integration<a name="cloudtrail-supported-services-application-services"></a>

 **Amazon Simple Notification Service**   
 Amazon Simple Notification Service \(Amazon SNS\) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\. For more information, see the [Amazon Simple Notification Service Developer Guide](http://docs.aws.amazon.com/sns/latest/dg/)\. For information about the Amazon SNS calls logged by CloudTrail, see [Logging Amazon Simple Notification Service API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/sns/latest/dg/logging-using-cloudtrail.html)\.  
Support began 10/09/2014

 **Amazon Simple Queue Service**   
Amazon Simple Queue Service \(Amazon SQS\) offers reliable and scalable hosted queues for storing messages as they travel between computers\. By using Amazon SQS, you can move data between distributed components of your applications that perform different tasks without losing messages or requiring each component to be always available\. For more information, see the [Amazon Simple Queue Service Developer Guide](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/)\. For information about the Amazon SQS calls logged by CloudTrail, see [Logging Amazon SQS API Actions Using AWS CloudTrail](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/logging-using-cloudtrail.html)\.   
Support began 07/16/2014

 **Amazon Simple Workflow Service**   
Amazon Simple Workflow Service \(Amazon SWF\) makes it easy to build applications that coordinate work across distributed components\. Amazon SWF gives you full control over implementing tasks and coordinating them without worrying about underlying complexities such as tracking their progress and maintaining their state\. For more information, see the [Amazon Simple Workflow Service Developer Guide](http://docs.aws.amazon.com/amazonswf/latest/developerguide/)\. For information about the Amazon SWF calls logged by CloudTrail, see [Logging Amazon Simple Workflow Service API Calls with AWS CloudTrail](http://docs.aws.amazon.com/amazonswf/latest/developerguide/ct-logging.html)\.   
Support began 05/13/2014

 **AWS Step Functions**   
AWS Step Functions enables you to coordinate a network of computing resources across distributed components using state machines\. You define state machines consisting of states that perform units of work, or tasks\. Tasks are invocations of your resources\. They report their results, along with success, failure, or heartbeat notifications back to Step Functions\. By following the logic flow expressed in your state machine definition, Step Functions coordinates when tasks are run, and passes data between the tasks\. For more information, see the [AWS Step Functions Developer Guide](http://docs.aws.amazon.com/step-functions/latest/dg/)\. For information about the AWS Step Functions calls logged by CloudTrail, see [Logging AWS Step Functions API Calls with AWS CloudTrail](http://docs.aws.amazon.com/step-functions/latest/dg/cloud-trail.html)\.  
Support began 12/01/2016

## Business Productivity<a name="cloudtrail-supported-services-business-productivity"></a>

 **Alexa for Business**  
Alexa for Business gives you the tools you need to manage Alexa devices, enroll your users, and assign skills, at scale\.   
For more information, see the [Alexa for Business Administration Guide](http://docs.aws.amazon.com/a4b/latest/ag/what-is.html)\. For information about the Alexa for Business actions logged by CloudTrail, see [Logging Alexa for Business Administration Calls Using AWS CloudTrail](http://docs.aws.amazon.com/a4b/latest/ag/cloudtrail.html)\.  
Support began 11/29/2017

 **Amazon Chime**  
Amazon Chime is a secure, real\-time, unified communications service that helps you run online meetings efficiently\. Amazon Chime runs securely on the AWS cloud\.   
For more information, see [Amazon Chime Administrator Guide](http://docs.aws.amazon.com/chime/latest/ag/)\. For information about the administrative Amazon Chime actions logged by CloudTrail, see [Log Amazon Chime Administration Calls Using AWS CloudTrail](http://docs.aws.amazon.com/chime/latest/ag/cloudtrail.html)\.   
Support began 09/27/2017

 **Amazon WorkDocs**  
Amazon WorkDocs is a fully managed enterprise storage and sharing service\. Your files are stored in the cloud safely and securely\.   
For more information, see [Amazon WorkDocs Administration Guide](http://docs.aws.amazon.com/workdocs/latest/adminguide/)\. For information about the Amazon WorkDocs actions logged by CloudTrail, see [Logging Amazon WorkDocs API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/workdocs/latest/adminguide/cloudtrail_logging.html)\.  
Support began 08/27/2014

 **Amazon WorkMail**  
Amazon WorkMail is a secure, managed business email and calendaring service with support for existing desktop and mobile email clients\.   
For more information, see [Amazon WorkMail Administrator Guide](http://docs.aws.amazon.com/workmail/latest/adminguide/)\. For information about the Amazon WorkMail actions logged by CloudTrail, see [Logging Amazon WorkMail API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/workmail/latest/adminguide/logging-using-cloudtrail.html)\.  
Support began 12/12/2017

## Compute<a name="cloudtrail-supported-services-compute"></a>

 **Application Auto Scaling**   
With Application Auto Scaling, you can automatically scale your AWS resources\. The experience is similar to that of Amazon EC2 Auto Scaling\. You can use Application Auto Scaling to define scaling policies to automatically scale your AWS resources, scale your resources in response to Amazon CloudWatch alarms, and view the history of your scaling events\. For more information, see the [Application Auto Scaling API Reference](http://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/)\. For information about the Application Auto Scaling calls logged by CloudTrail, see [Logging Application Auto Scaling API calls with AWS CloudTrail](http://docs.aws.amazon.com/ApplicationAutoScaling/latest/APIReference/logging-using-cloudtrail.html)  
Support began 10/31/2016

 **Amazon EC2 Auto Scaling**   
Auto Scaling is a web service that enables you to automatically launch or terminate Amazon Elastic Compute Cloud \(Amazon EC2\) instances based on user\-defined policies, health status checks, and schedules\. For more information, see the [Amazon EC2 Auto Scaling User Guide](http://docs.aws.amazon.com/autoscaling/ec2/userguide/)\. For information about the Auto Scaling calls logged by CloudTrail, see [Logging Auto Scaling API Calls By Using CloudTrail](http://docs.aws.amazon.com/autoscaling/ec2/userguide/logging-using-cloudtrail.html)\.  
Support began 07/16/2014

**AWS Batch**  
AWS Batch enables you to run batch computing workloads on the AWS Cloud\. Batch computing is a common way for developers, scientists, and engineers to access large amounts of compute resources, and AWS Batch removes the undifferentiated heavy lifting of configuring and managing the required infrastructure\. For more information, see the [AWS Batch User Guide](http://docs.aws.amazon.com/batch/latest/userguide/)\. For more information about the AWS Batch APIs logged by CloudTrail see [Logging AWS Batch API Calls with AWS CloudTrail](http://docs.aws.amazon.com/batch/latest/userguide/logging-using-cloudtrail.html)\.  
Support began 1/10/2018

 **Amazon Elastic Container Registry**   
Amazon Elastic Container Registry \(Amazon ECR\) is a secure and scalable managed AWS Docker registry service\. Amazon ECR supports private Docker repositories with resource\-level permissions\. You can use the Docker CLI to author, push, pull, and manage images\. For more information, see the [Amazon Elastic Container Registry User Guide](http://docs.aws.amazon.com/AmazonECR/latest/userguide/)\. For information about the Amazon ECR calls logged by CloudTrail, see [ Logging Amazon ECR API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonECR/latest/userguide/logging-using-cloudtrail.html)\.   
Support began 12/21/2015

 **Amazon Elastic Container Service**   
Amazon Elastic Container Service \(Amazon ECS\) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster of Amazon EC2 instances\. For more information, see the [Amazon Elastic Container Service Developer Guide](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/)\. For information about the Amazon ECS calls logged by CloudTrail, see [Logging Amazon ECS API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/logging-using-cloudtrail.html)\.   
Support began 04/09/2015

 **AWS Elastic Beanstalk**   
 You can use Elastic Beanstalk to quickly deploy and manage applications in the AWS cloud without worrying about the infrastructure that runs those applications\. For more information, see the [AWS Elastic Beanstalk Developer Guide](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/)\. For information about the Elastic Beanstalk calls logged by CloudTrail, see [Using AWS Elastic Beanstalk with AWS CloudTrail](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudtrail.html)\.  
Support began 03/31/2014

 **Amazon Elastic Compute Cloud**   
Amazon EC2 \(Amazon EC2\) provides resizeable computing capacity in the AWS cloud\. You can launch as many or as few virtual servers as you need, configure security and networking, and manage storage\. Amazon EC2 can also scale up or down quickly to handle changes in requirements or spikes in popularity, thereby reducing your need to forecast server traffic\.   
For more information, see the [Amazon EC2 User Guide for Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)\. For information about the Amazon EC2 calls logged by CloudTrail, see [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html)\.   
[Amazon EC2 Systems Manager \(SSM\)](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-configuration-manage.html) is a feature of [EC2Config](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html) that enables you to manage the configuration of running Windows instances\. For information about the Amazon EC2 Systems Manager API calls logged by CloudTrail, see [Logging SSM API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/ssm/latest/APIReference/logging-using-cloudtrail.html)\.   
Support began 11/13/2013

 **Elastic Load Balancing**   
You can use Elastic Load Balancing to automatically distribute your incoming application traffic across multiple Amazon EC2 instances\. Elastic Load Balancing automatically scales request handling capacity in response to incoming traffic\. For more information, see the [Elastic Load Balancing User Guide](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/)\. For information about the Elastic Load Balancing calls logged by CloudTrail, see [AWS CloudTrail Logging for Your Classic Load Balancer](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/ELB-API-Logs.html) and [AWS CloudTrail Logging for Your Application Load Balancer](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudtrail-logs.html)\.  
Support began 04/04/2014

 **AWS Lambda**   
AWS Lambda is a zero\-administration compute platform that runs your code in the AWS Cloud, providing the high availability, security, performance, and scalability of AWS infrastructure\. For more information, see the [AWS Lambda Developer Guide](http://docs.aws.amazon.com/lambda/latest/dg/)\. For information about the Lambda calls logged by CloudTrail, see [Logging AWS Lambda API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/lambda/latest/dg/logging-using-cloudtrail.html)\.   
You can also configure a trail to log data events for the `Invoke` API\. For more information, see [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.   
Support began 04/09/2015 for management events  
Support began 11/30/2017 for data events

 **Amazon Lightsail**   
Amazon Lightsail helps developers quickly get started with virtual private servers\. Lightsail includes everything you need to launch your project quickly – a virtual machine, SSD\-based storage, data transfer, DNS management, and a static IP\. For more information, see the [Amazon Lightsail Developer Guide](https://lightsail.aws.amazon.com/ls/docs/)\. For information about the Lightsail calls logged by CloudTrail, see [Logging Lightsail API Calls with AWS CloudTrail](https://lightsail.aws.amazon.com/ls/docs/how-to/article/logging-lightsail-api-calls-using-aws-cloudtrail)\.   
Support began 12/23/2016

## Customer Engagement<a name="cloudtrail-supported-services-customer-engagement"></a>

 **Amazon Simple Email Service**   
Amazon Simple Email Service is an outbound\-only email\-sending service that provides an easy, cost\-effective way for you to send email\. For more information, see the [Amazon Simple Email Service Developer Guide](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/)\. For information about the Amazon SES calls logged by CloudTrail, see [Logging Amazon SES API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/ses/latest/DeveloperGuide/logging-using-cloudtrail.html)\.  
Support began 05/07/2015

## Database<a name="cloudtrail-supported-services-database"></a>

 **Amazon DynamoDB**   
Amazon DynamoDB is a fully managed NoSQL database service that provides fast and predictable performance with seamless scalability\. For more information, see the [Amazon DynamoDB Developer Guide](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)\. For information about the DynamoDB calls logged by CloudTrail, see [Logging DynamoDB Operations By Using AWS CloudTrail](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/logging-using-cloudtrail.html)\.  
Support began 05/28/2015

 **Amazon ElastiCache**   
Amazon ElastiCache is a web service that makes it easy to set up, manage, and scale distributed in\-memory cache environments in the cloud\. It provides a high performance, resizeable, and cost\-effective in\-memory cache, while removing the complexity associated with deploying and managing a distributed cache environment\. For more information, see the [Amazon ElastiCache User Guide](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/)\. For information about the ElastiCache calls logged by CloudTrail, see [Logging Amazon ElastiCache API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Logging.html)\.  
Support began 09/15/2014

 **Amazon Redshift**   
Amazon Redshift is a fast, fully managed, petabyte\-scale data warehouse service that makes it simple and cost\-effective to efficiently analyze all your data by using your existing business intelligence tools\. It is optimized for datasets that range from a few hundred gigabytes to a petabyte or more\. For more information, see the [Amazon Redshift Database Developer Guide](http://docs.aws.amazon.com/redshift/latest/dg/)\. For more information about Amazon Redshift API calls logged by CloudTrail, see [Using AWS CloudTrail for Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html#rs-db-auditing-cloud-trail)\. For the list of Amazon Redshift calls logged by CloudTrail, see the [Amazon Redshift API Reference](http://docs.aws.amazon.com/redshift/latest/APIReference/)\.  
Support began 06/10/2014

 **Amazon Relational Database Service**   
Amazon Relational Database Service \(Amazon RDS\) is a web service that makes it easier to set up, operate, and scale a relational database in the cloud\. It provides cost\-efficient, resizeable capacity for an industry\-standard relational database and manages common database administration tasks\. For more information, see the [Amazon Relational Database Service User Guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/)\. For information about the Amazon RDS calls logged by CloudTrail, see [Logging Amazon RDS API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Auditing.html)\.   
Support began 11/13/2013

## Desktop & App Streaming<a name="cloudtrail-supported-services-desktop-and-app-streaming"></a>

 **Amazon WorkSpaces**   
Amazon WorkSpaces offers an easy way to provide a cloud\-based desktop experience to your end\-users\. A choice of bundles offer a range of different amounts of CPU, memory, storage, and a choice of applications\. Users can connect from a PC, Mac desktop computer, iPad, Kindle, or Android tablet\. For more information, see the [Amazon WorkSpaces Administration Guide](http://docs.aws.amazon.com/workspaces/latest/adminguide/)\. For information about the Amazon WorkSpaces actions logged by CloudTrail, see [Logging Amazon WorkSpaces API Calls by Using CloudTrail](http://docs.aws.amazon.com/workspaces/latest/devguide/cloudtrail_logging.html)\.  
Support began 04/09/2015

## Developer Tools<a name="cloudtrail-supported-services-developer-tools"></a>

 **AWS CodeBuild**   
AWS CodeBuild is a fully managed build service in the cloud\. AWS CodeBuild compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. For more information, see the [AWS CodeBuild User Guide](http://docs.aws.amazon.com/codebuild/latest/userguide/)\. For information about the AWS CodeBuild calls logged by CloudTrail, see [Logging AWS CodeBuild API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codebuild/latest/userguide/cloudtrail.html)\.  
Support began 12/01/2016

 **AWS CodeCommit**   
AWS CodeCommit is a version control service hosted by AWS that you can use to privately store and manage assets \(such as documents, source code, and binary files\) in the cloud\. For more information, see the [AWS CodeCommit User Guide](http://docs.aws.amazon.com/codecommit/latest/userguide/)\. For information about the AWS CodeCommit calls logged by CloudTrail, see [Logging AWS CodeCommit API Calls with AWS CloudTrail](http://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)\.   
Support began 01/11/2017

 **AWS CodeDeploy**   
AWS CodeDeploy is a deployment service that enables developers to automate the deployment of applications to Amazon Elastic Compute Cloud \(Amazon EC2\) instances, and to update the applications as required\. For more information, see the [AWS CodeDeploy User Guide](http://docs.aws.amazon.com/codedeploy/latest/userguide/)\. For information about the AWS CodeDeploy calls logged by CloudTrail, see [Monitoring Deployments with AWS CloudTrail](http://docs.aws.amazon.com/codedeploy/latest/userguide/cloudtrail-integ.html)\.   
Support began 12/16/2014

 **AWS CodePipeline**   
AWS CodePipeline is a continuous delivery and automation service hosted by Amazon Web Services that enables you to model, configure, and automate the steps required to release your software\. For more information, see the [AWS CodePipeline User Guide](http://docs.aws.amazon.com/codepipeline/latest/userguide/)\. For information about the AWS CodePipeline calls logged by CloudTrail, see [Logging AWS CodePipeline API Calls with AWS CloudTrail\.](http://docs.aws.amazon.com/codepipeline/latest/userguide/integ-cloudtrail.html)   
Support began 07/09/2015

 **AWS CodeStar**   
AWS CodeStar is a cloud\-based service for creating, managing, and working with software development projects on AWS\. You can develop, build, and deploy applications on AWS with an AWS CodeStar project\. An AWS CodeStar project creates and integrates AWS services for your project development toolchain\. For more information, see the [AWS CodeStar User Guide](http://docs.aws.amazon.com/codestar/latest/userguide/)\. For information about the AWS CodeStar calls logged by CloudTrail, see [Logging AWS CodeStar API Calls with AWS CloudTrail\.](http://docs.aws.amazon.com/codestar/latest/userguide/cloudtrail.html)   
Support began 06/14/2017

 **AWS X\-Ray**   
AWS X\-Ray is a service that collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization\. For more information, see the [AWS X\-Ray Developer Guide](http://docs.aws.amazon.com/xray/latest/devguide/)\. For information about the AWS CodeStar calls logged by CloudTrail, see [Logging AWS X\-Ray API Calls With CloudTrail](http://docs.aws.amazon.com/xray/latest/devguide/xray-api-cloudtrail.html)\.  
Support began 04/25/2018

## Game Development<a name="cloudtrail-supported-services-game-development"></a>

 **Amazon GameLift**   
Amazon GameLift is a fully managed service for deploying, operating, and scaling session\-based multiplayer game servers in the cloud\. You can deploy your first game server in the cloud in just minutes, eliminating up to thousands of hours in upfront software development\. For more information about GameLift, see the [Amazon GameLift Developer Guide](http://docs.aws.amazon.com/gamelift/latest/developerguide/)\. For information about the GameLift calls logged by CloudTrail, see [Logging Amazon GameLift API Calls with AWS CloudTrail](http://docs.aws.amazon.com/gamelift/latest/developerguide/logging-using-cloudtrail.html)\.  
Support began 01/27/2016

## Internet of Things<a name="cloud-trail-supported-services-internet-of-things"></a>

 **AWS IoT**   
AWS IoT provides secure, bi\-directional communication between Internet\-connected things \(such as sensors, actuators, embedded devices, or smart appliances\) and the AWS cloud\. This enables you to collect telemetry data from multiple devices and store and analyze the data\. **AWS IoT Device Management** is a cloud\-based device management service that makes it easy for customers to securely manage IoT devices throughout their lifecycle\. For more information about AWS IoT and AWS IoT Device Management, see the [AWS IoT Developer Guide](http://docs.aws.amazon.com/iot/latest/developerguide/)\. For information about the AWS IoT and AWS IoT Device Management calls logged by CloudTrail, see [Logging AWS IoT API calls with AWS CloudTrail](http://docs.aws.amazon.com/iot/latest/developerguide/monitoring_overview.html#iot-using-cloudtrail)\.  
Support began 04/11/2016

 **AWS IoT Analytics**   
AWS IoT Analytics allows you to collect large amounts of device data, process messages, and store them\. You can then query the data and run sophisticated analytics on it\. For more information about AWS IoT Analytics, see the [AWS IoT Analytics User Guide](http://docs.aws.amazon.com/iotanalytics/latest/userguide/welcome.html)\. For information about the AWS IoT calls logged by CloudTrail, see [Logging AWS IoT Analytics API calls with AWS CloudTrail](http://docs.aws.amazon.com/iotanalytics/latest/userguide/cloudtrail.html)\.  
Support began 04/23/2018

## Machine Learning<a name="cloudtrail-supported-services-artificial-intelligence"></a>

 **Amazon Lex**   
Amazon Lex is an AWS service for building conversational interfaces for applications using voice and text\. With Amazon Lex, the same conversational engine that powers Amazon Alexa is now available to any developer\. You can use it to build sophisticated, natural language chatbots into your new and existing applications\. For more information, see the [Amazon Lex Developer Guide](http://docs.aws.amazon.com/lex/latest/dg/)\. For information about the Amazon Lex API calls logged by CloudTrail, see [Logging Amazon Lex API Calls with CloudTrail](http://docs.aws.amazon.com/lex/latest/dg/monitoring-aws-lex-cloudtrail.html)\.  
Support began 08/15/2017

 **Amazon Machine Learning**   
Amazon Machine Learning makes it easy for developers to build smart applications, including applications for fraud detection, demand forecasting, targeted marketing, and click prediction\. The powerful algorithms of Amazon Machine Learning create machine learning \(ML\) models by finding patterns in your existing data\. For more information, see the [Amazon Machine Learning Developer Guide](http://docs.aws.amazon.com/machine-learning/latest/dg/)\. For information about the Amazon ML API calls logged by CloudTrail, see [Logging Amazon ML API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/machine-learning/latest/dg/logging-using-cloudtrail.html)\.  
Support began 12/10/2015

 **Amazon Polly**   
Amazon Polly is a service that converts text into lifelike speech\. You can use Amazon Polly to develop applications that increase engagement and accessibility\. For more information, see the [Amazon Polly Developer Guide](http://docs.aws.amazon.com/polly/latest/dg/)\. For information about the Amazon Polly calls logged by CloudTrail, see [Logging Amazon Polly API Calls with AWS CloudTrail](http://docs.aws.amazon.com/polly/latest/dg/logging-using-cloudtrail.html)\.  
Support began 11/30/2016

 **Amazon Rekognition**   
Amazon Rekognition makes it easy to add image and video analysis to your applications\. You just provide an image or video to the Amazon Rekognition API, and the service can identify objects, people, text, scenes, and activities\. For more information, see the [Amazon Rekognition Developer Guide](http://docs.aws.amazon.com/rekognition/latest/dg/)\. For information about the Amazon Rekognition calls logged by CloudTrail, see [Logging Amazon Rekognition API Calls with AWS CloudTrail](http://docs.aws.amazon.com/rekognition/latest/dg/logging-using-cloudtrail.html)\.  
Support began 4/6/2018

 **Amazon SageMaker**   
Amazon SageMaker is a fully managed machine learning service\. With Amazon SageMaker, data scientists and developers can quickly and easily build and train machine learning models, and then directly deploy them into a production\-ready hosted environment\. For more information, see the [Amazon SageMaker Developer Guide](http://docs.aws.amazon.com/sagemaker/latest/dg/)\. For information about the Amazon SageMaker calls logged by CloudTrail, see [Logging Amazon SageMaker API Calls with AWS CloudTrail](http://docs.aws.amazon.com/sagemaker/latest/dg/logging-using-cloudtrail.html)\.  
Support began 1/11/2018

## Management Tools<a name="cloudtrail-supported-services-management-tools"></a>

 **AWS Application Discovery Service**   
AWS Application Discovery Service helps you plan application migration projects by automatically identifying servers, virtual machines \(VMs\), software, and software dependencies running in your on\-premises data centers\. For more information, see the [Application Discovery Service User Guide](http://docs.aws.amazon.com/application-discovery/latest/userguide/)\.  
All Application Discovery Service actions are logged\. For more information, see the [Application Discovery Service API Reference](http://docs.aws.amazon.com/application-discovery/latest/APIReference/)\.  
Support began 05/12/2016

 **AWS CloudFormation**   
AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. It helps you leverage AWS products such as Amazon EC2, Amazon EBS, Amazon SNS, Elastic Load Balancing, and Auto Scaling to build highly reliable, highly scalable, cost\-effective applications without worrying about creating and configuring the underlying AWS infrastructure\.   
For more information, see the [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/)\. For information about the AWS CloudFormation calls logged by CloudTrail, see [Logging AWS CloudFormation API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-api-logging-cloudtrail.html)\.  
Support began 04/02/2014

 **AWS CloudTrail**   
Like any supported service, when logging is turned on, CloudTrail logs actions to an Amazon S3 bucket that you specify\. All CloudTrail actions are logged\. See the [AWS CloudTrail API Reference](http://docs.aws.amazon.com/awscloudtrail/latest/APIReference/)\.  
Support began 11/13/2013

 **Amazon CloudWatch**   
Amazon CloudWatch monitors your AWS resources and the applications you run on AWS\. You can use CloudWatch to collect and track metrics which are the variables you want to measure for your resources and applications\. CloudWatch alarms send notifications or automatically make changes to the resources you are monitoring based on rules that you define\. For more information, see the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\. For information about the CloudWatch calls logged by CloudTrail, see [Logging Amazon CloudWatch API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/logging_cw_api_calls.html)\.   
Support began 04/30/2014

 **Amazon CloudWatch Events**   
Amazon CloudWatch Events delivers a timely stream of system events that describe changes in AWS resources to AWS Lambda functions, streams in Amazon Kinesis Data Streams, Amazon SNS topics, or built\-in targets\. Using simple rules that you can set up quickly, you can match events and route them to one or more target functions or streams\. For more information, see the [Amazon CloudWatch Events User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)\. For information about the CloudWatch calls logged by CloudTrail, see [Logging Amazon CloudWatch Events API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/logging_cw_api_calls_cwe.html)\.   
Support began 01/16/2016

 **Amazon CloudWatch Logs**   
Amazon CloudWatch Logs monitors, stores, and accesses your log files from Amazon EC2 instances, AWS CloudTrail, and other sources\. You can then retrieve the associated log data from CloudWatch Logs\. For more information, see the [Amazon CloudWatch Logs User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)\. For information about the CloudWatch Logs calls logged by CloudTrail, see [Logging Amazon CloudWatch Logs API Calls in AWS CloudTrail](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/logging_cw_api_calls_cwl.html)\.  
Support began 03/10/2016

 **AWS Config**   
AWS Config provides a detailed view of the resources associated with your AWS account, including how they are configured, how they are related to one another, and how the configurations and their relationships have changed over time\. For more information, see the [AWS Config Developer Guide](http://docs.aws.amazon.com/config/latest/developerguide/)\. For information about the AWS Config calls logged by CloudTrail, see [Logging AWS Config API Calls with AWS CloudTrail](http://docs.aws.amazon.com/config/latest/developerguide/log-api-calls.html)\.  
Support began 02/10/2015

 **AWS Managed Services**   
AWS Managed Services provides ongoing management of your AWS infrastructure so you can focus on your applications\. By implementing best practices to maintain your infrastructure, AWS Managed Services helps to reduce your operational overhead and risk\. For more information, see [AWS Managed Services](https://aws.amazon.com/managed-services)\.  
Support began 12/21/2016

 **AWS OpsWorks**   
AWS OpsWorks provides a simple and flexible way to create and manage stacks and applications\. It supports a standard set of components—including application servers, database servers, load balancers, and more—that you can use to assemble your stack\. These components all come with a standard configuration and are ready to run\. For more information, see the [AWS OpsWorks User Guide](http://docs.aws.amazon.com/opsworks/latest/userguide/)\. For information about the AWS OpsWorks calls logged by CloudTrail, see [Logging AWS OpsWorks API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/opsworks/latest/userguide/monitoring-cloudtrail.html)\.   
Support began 06/04/2014

 **AWS OpsWorks for Chef Automate**   
AWS OpsWorks for Chef Automate lets you run a Chef Automate server in AWS\. You can provision a Chef server within minutes, and let AWS OpsWorks Stacks handle its operations, backups, restorations, and software upgrades\. For more information, see the [AWS OpsWorks User Guide](http://docs.aws.amazon.com/opsworks/latest/userguide/)\. For information about the AWS OpsWorks for Chef Automate calls logged by CloudTrail, see [Logging AWS OpsWorks for Chef Automate API Calls with AWS CloudTrail](http://docs.aws.amazon.com/opsworks/latest/userguide/logging-using-cloudtrail.html)\.  
Support began 11/23/2016

 **AWS Organizations**   
AWS Organizations is an account management service that enables you to consolidate multiple AWS accounts into an organization that you create and centrally manage\. For more information, see the [AWS Organizations User Guide](http://docs.aws.amazon.com/organizations/latest/userguide/)\. For information about the AWS Organizations calls logged by CloudTrail, see [Monitor the Activity in Your Organization](http://docs.aws.amazon.com/organizations/latest/userguide/orgs_monitoring.html#orgs_cloudtrail-integration)\.   
Support began 02/27/2017

 **AWS Service Catalog**   
AWS Service Catalog allows organizations to create and manage catalogs of IT services that are approved for use on AWS\. These IT services can include everything from virtual machine images, servers, software, and databases to complete multi\-tier application architectures\. For more information, see the [AWS Service Catalog Developer Guide](http://docs.aws.amazon.com/servicecatalog/latest/dg/)\. For information about the AWS Service Catalog calls logged by CloudTrail, see [Logging AWS Service Catalog API Calls with AWS CloudTrail](http://docs.aws.amazon.com/servicecatalog/latest/dg/logging-using-cloudtrail.html)\.  
Support began 07/06/2016

## Media Services<a name="cloudtrail-supported-services-media-services"></a>

 **Amazon Elastic Transcoder**   
Amazon Elastic Transcoder lets you convert media files that you have stored in Amazon S3 into media files in the formats required by consumer playback devices\. For more information, see the [Amazon Elastic Transcoder Developer Guide](http://docs.aws.amazon.com/elastictranscoder/latest/developerguide/)\. For information about the Elastic Transcoder calls logged by CloudTrail, see [Logging Elastic Transcoder API Calls Using CloudTrail\.](http://docs.aws.amazon.com/elastictranscoder/latest/developerguide/logging_using_cloudtrail.html)   
Support began 10/27/2014

 **AWS Elemental MediaConvert**   
AWS Elemental MediaConvert is a file\-based video processing service that provides scalable video processing for content owners and distributors with media libraries of any size\. For more information, see the [AWS Elemental MediaConvert User Guide](http://docs.aws.amazon.com/mediaconvert/latest/ug/what-is.html)\. For information about the AWS Elemental MediaConvert calls logged by CloudTrail, see [Logging AWS Elemental MediaConvert API Calls with CloudTrail](http://docs.aws.amazon.com/mediaconvert/latest/ug/logging-using-cloudtrail.html)\.  
Support began 11/27/2017

 **AWS Elemental MediaStore**   
AWS Elemental MediaStore is a video origination and storage service that offers the high performance and immediate consistency required for live and on\-demand media\. For more information, see the [AWS Elemental MediaStore User Guide](http://docs.aws.amazon.com/mediastore/latest/ug/what-is.html)\. For information about the AWS Elemental MediaStore calls logged by CloudTrail, see [Logging AWS Elemental MediaStore API Calls with CloudTrail](http://docs.aws.amazon.com/mediastore/latest/ug/logging-using-cloudtrail.html)\.   
Support began 11/27/2017

## Migration<a name="cloudtrail-supported-services-migration"></a>

 **AWS Database Migration Service**   
AWS Database Migration Service \(AWS DMS\) can migrate your data to and from most widely used commercial and open\-source databases such as Oracle, PostgreSQL, Microsoft SQL Server, Amazon Aurora, MariaDB, and MySQL\. For more information, see the [AWS Database Migration Service User Guide](http://docs.aws.amazon.com/dms/latest/userguide/)\. For information about the AWS DMS calls logged by CloudTrail, see [Logging AWS Database Migration Service API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/dms/latest/userguide/CHAP_Monitoring.CloudTrail.html)\.   
Support began 02/04/2016

 **AWS Migration Hub**   
AWS Migration Hub helps you have a detailed understanding of your servers\. It collects data about your on\-premises environment via AWS discovery tools such as the AWS Application Discovery Agent and the AWS Agentless Discovery Connector\. For more information, see the [AWS Migration Hub User Guide](http://docs.aws.amazon.com/migrationhub/latest/ug/whatishub.html)\. For information about the AWS Migration Hub calls logged by CloudTrail, see [Logging AWS Migration Hub API Calls with AWS CloudTrail](http://docs.aws.amazon.com/migrationhub/latest/ug/logging-using-cloudtrail.html)\.  
Support began 08/14/2017

 **AWS Server Migration Service**   
AWS Server Migration Service \(AWS SMS\) automates the migration of on\-premises VMware virtual machines to the AWS Cloud and Amazon EC2\. AWS SMS incrementally replicates your server VMs as cloud\-hosted Amazon Machine Images \(AMIs\)\. Working with AMIs, you can test and update your replicated, cloud\-based VMs before deploying them in production\. All AWS SMS actions are logged\. See the [AWS SMS API Reference](http://docs.aws.amazon.com/ServerMigration/latest/APIReference/)\.  
Support began 11/14/2016

## Mobile Services<a name="cloudtrail-supported-services-mobile-services"></a>

 **AWS AppSync**   
AWS AppSync is an enterprise\-level, fully managed GraphQL service with real\-time data synchronization and offline programming features\. For more information, see the [AWS AppSync Developer Guide](http://docs.aws.amazon.com/appsync/latest/devguide/)\. For information about the AWS AppSync calls logged by CloudTrail, see [Logging AWS AppSync API Calls with AWS CloudTrail\.](http://docs.aws.amazon.com/appsync/latest/devguide/cloudtrail-logging.html)\.   
Support began 02/13/2018

 **Amazon Cognito**   
Amazon Cognito is a service that you can use to create unique identities for your users, authenticate these identities with identity providers, and save mobile user data in the AWS Cloud\. For more information, see the [Amazon Cognito Developer Guide](http://docs.aws.amazon.com/cognito/latest/developerguide/)\. For information about the Amazon Cognito calls logged by CloudTrail, see [Logging Amazon Cognito API Calls with AWS CloudTrail\.](http://docs.aws.amazon.com/cognito/latest/developerguide/logging-using-cloudtrail.html)\.   
Support began 02/18/2016

 **AWS Device Farm**   
AWS Device Farm is an app testing service that enables you to test your Android and Fire OS apps on real, physical phones and tablets that are hosted by AWS\. For more information, see the [Device Farm Developer Guide](http://docs.aws.amazon.com/devicefarm/latest/developerguide/)\. For information about the AWS Device Farm calls logged by CloudTrail, see [Logging AWS Device Farm API Calls By Using AWS CloudTrail\.](http://docs.aws.amazon.com/devicefarm/latest/developerguide/cloudtrail.html)   
Support began 07/13/2015

 **Amazon Pinpoint**   
Amazon Pinpoint is an AWS service that you can use to engage with your customers across multiple messaging channels\. For more information, see the [Amazon Pinpoint Developer Guide](http://docs.aws.amazon.com/pinpoint/latest/developerguide/)\. For information about the AWS Device Farm calls logged by CloudTrail, see [Logging Amazon Pinpoint API Calls with AWS CloudTrail](http://docs.aws.amazon.com/pinpoint/latest/developerguide/logging-using-cloudtrail.html)\.   
Support began 02/06/2018

## Networking & Content Delivery<a name="cloudtrail-supported-services-networking"></a>

 **Amazon API Gateway**   
Amazon API Gateway helps developers deliver robust, reliable, secure and scalable access to backend APIs for mobile apps, web apps, and server apps\. For more information, see the [API Gateway Developer Guide](http://docs.aws.amazon.com/apigateway/latest/developerguide/)\. For information about the Amazon API Gateway calls logged by CloudTrail, see [Log API management calls to Amazon API Gateway Using AWS CloudTrail](http://docs.aws.amazon.com/apigateway/latest/developerguide/cloudtrail.html)\.  
Support began 07/09/2015

 **Amazon CloudFront**   
Amazon CloudFront speeds up distribution of your static and dynamic web content to end users\. CloudFront delivers your content through a worldwide network of edge locations\. When an end user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency, so that content is delivered with the best possible performance\. For more information, see the [Amazon CloudFront Developer Guide](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/)\. For information about the CloudFront calls logged by CloudTrail, see [Using AWS CloudTrail to Capture Requests Sent to the CloudFront API](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/logging_using_cloudtrail.html)\.  
Support began 05/28/2014

 **AWS Direct Connect**   
You can use AWS Direct Connect to establish a direct connection from your premises to AWS\. This may reduce your network costs and increase bandwidth throughput\. For more information, see the [AWS Direct Connect User Guide](http://docs.aws.amazon.com/directconnect/latest/UserGuide/)\. For information about the AWS Direct Connect calls logged by CloudTrail, see [Logging AWS Direct Connect API Calls in AWS CloudTrail](http://docs.aws.amazon.com/directconnect/latest/UserGuide/logging_dc_api_calls.html)\.  
Support began 03/08/2014

 **Amazon Route 53**   
Amazon Route 53 is a Domain Name System \(DNS\), domain name registration, and automatic endpoint naming web service\. To use Amazon Route 53 with CloudTrail, you must choose US East \(N\. Virginia\) as the region when you create the trail\. For more information, see the [Amazon Route 53 Developer Guide](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/)\. For information about the Amazon Route 53 calls logged by CloudTrail, see [Using AWS CloudTrail to Capture Requests Sent to the Route 53 API](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/logging-using-cloudtrail.html)\.   
Support began 02/11/2015

 **Amazon Virtual Private Cloud**   
Amazon Virtual Private Cloud \(Amazon VPC\) enables you to launch AWS resources into a virtual network that you've defined\. This virtual network closely resembles a traditional network that you would operate in your own data center with the added benefit of using the scalable AWS infrastructure\. For more information, see the [Amazon VPC User Guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/)\. The Amazon VPC API is a subset of the Amazon EC2 API\. For information about the Amazon EC2 calls logged by CloudTrail \(including those for Amazon VPC\), see [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html)\.  
Support began 11/13/2013

## Security, Identity & Compliance<a name="cloudtrail-supported-services-security-and-identity"></a>

 **AWS Certificate Manager**   
AWS Certificate Manager \(ACM\) handles the complexity of provisioning, deploying, and managing certificates provided by ACM \(ACM Certificates\) for your AWS\-based websites and applications\. For more information, see the [AWS Certificate Manager User Guide](http://docs.aws.amazon.com/acm/latest/userguide/)\. For information about the ACM calls logged by CloudTrail, see [Using AWS CloudTrail](http://docs.aws.amazon.com/acm/latest/userguide/cloudtrail.html)\.  
Support began 03/25/2016

 **Amazon Cloud Directory**   
Amazon Cloud Directory is a highly scalable, high performance, multitenant directory service in the cloud\. Its web\-based directories make it easy for you to organize and manage application resources such as users, groups, locations, devices, policies, and the rich relationships between them\. Cloud Directory is a foundational building block for developers to create directory\-based solutions easily and without having to worry about deployment, global scale, availability, and performance\. For more information, see [Amazon Cloud Directory](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_amazon_cd.html) in the *AWS Directory Service Administration Guide*\.   
For information about the Amazon Cloud Directory calls logged by CloudTrail, see [Logging Cloud Directory API calls Using AWS CloudTrail](http://docs.aws.amazon.com/amazoncds/latest/APIReference/cloudtrail_logging.html)\.  
Support began 01/26/2017

 **AWS CloudHSM**   
AWS CloudHSM provides secure cryptographic key storage to customers by making hardware security modules \(HSMs\) available in the AWS cloud\. For more information, see the [AWS CloudHSM User Guide](http://docs.aws.amazon.com/cloudhsm/latest/userguide/)\. For information about the AWS CloudHSM calls logged by CloudTrail, see [Logging AWS CloudHSM API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/cloudhsm/latest/userguide/cloudtrail_logging.html)\.   
Support began 01/08/2015

 **AWS Directory Service**   
The AWS Directory Service is a managed service that makes it easy to connect to your existing on\-premises Microsoft Active Directory and deploy and manage Windows workloads in the AWS cloud\. For more information, see the [AWS Directory Service Administration Guide](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\. For information about the AWS Directory Service calls logged by CloudTrail, see [Logging AWS Directory Service API Calls by Using CloudTrail](http://docs.aws.amazon.com/directoryservice/latest/devguide/cloudtrail_logging.html)\.  
Support began 05/14/2015

 **AWS Identity and Access Management**   
AWS Identity and Access Management \(IAM\) is a web service that enables AWS customers to manage users and user permissions\. By using IAM, you can centrally manage users, security credentials such as access keys, and permissions that control which AWS resources users can access\. For more information, see the [IAM User Guide](http://docs.aws.amazon.com/IAM/latest/UserGuide/)\. For information about the IAM calls logged by CloudTrail, see [Logging IAM Events with AWS CloudTrail](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html)\.   
Support began 11/13/2013

 **Amazon Inspector**   
Amazon Inspector enables you to analyze the behavior of your AWS resources and helps you to identify potential security issues\. With Amazon Inspector, you can define a collection of AWS resources that you want to include in an assessment target\. For more information, see the [Amazon Inspector User Guide](http://docs.aws.amazon.com/inspector/latest/userguide/)\. For information about the Amazon Inspector calls logged by CloudTrail, see [Logging Amazon Inspector API calls with AWS CloudTrail](http://docs.aws.amazon.com/inspector/latest/userguide/logging-using-cloudtrail.html)\.  
Support began 04/20/2016

**Amazon GuardDuty**  
Amazon GuardDuty is a continuous security monitoring service that analyzes and processes the following data sources: VPC Flow Logs, AWS CloudTrail event logs, and DNS logs\. For more information, see the [Amazon GuardDuty User Guide](http://docs.aws.amazon.com/guardduty/latest/ug/)\.  
Support began 02/12/2018

 **AWS Key Management Service**   
AWS Key Management Service is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data\. AWS KMS is integrated with other AWS services including Amazon EBS, Amazon S3, and Amazon Redshift\. For more information, see the [AWS Key Management Service Developer Guide](http://docs.aws.amazon.com/kms/latest/developerguide/)\. For information about the AWS KMS calls logged by CloudTrail, see [Logging AWS KMS API Calls](http://docs.aws.amazon.com/kms/latest/developerguide/logging-using-cloudtrail.html)\.  
Support began 11/12/2014

 **AWS Private Certificate Authority \(PCA\)**   
AWS Private Certificate Authority \(PCA\) is a hosted private certificate authority service for issuing and revoking private digital certificates\. For more information, see the [AWS Private Certificate Authority \(PCA\) User Guide](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)\. For information about the calls logged by CloudTrail, see [Using CloudTrail](http://docs.aws.amazon.com/acm-pca/latest/userguide/PcaCtIntro.html)\.  
Support began 4/4/2018

 **AWS Secrets Manager**   
AWS Secrets Manager helps you to securely encrypt, store, and retrieve credentials for your databases and other services\. For more information, see the [AWS Secrets Manager User Guide](http://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)\. For information about the calls logged by CloudTrail, see [Monitor the Use of Your AWS Secrets Manager Secrets](http://docs.aws.amazon.com/secretsmanager/latest/userguide/monitoring.html#monitoring_cloudtrail)\.  
Support began 4/5/2018

 **AWS Security Token Service**   
You can use the AWS Security Token Service \(AWS STS\) to grant a trusted user temporary, limited access to your AWS resources\. For more information, see [Temporary Security Credentials](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html) in the *IAM User Guide*\. For information about both the IAM and AWS STS calls logged by CloudTrail, see [Logging IAM Events with AWS CloudTrail](http://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-integration.html)\. For a complete list of AWS STS calls, see the [AWS Security Token Service API Reference](http://docs.aws.amazon.com/STS/latest/APIReference/)\.   
Support began 11/13/2013

**AWS Shield**  
AWS Shield is a managed Distributed Denial of Service \(DDoS\) protection service that safeguards web applications running on AWS\. For more information, see [AWS WAF Developer Guide](http://docs.aws.amazon.com/waf/latest/developerguide/)\. For information about the AWS Shield calls logged by CloudTrail, see [Logging Shield Advanced API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-shield-using-cloudtrail.html)\.  
Support began 2/8/2018

**AWS Single Sign\-On \(AWS SSO\)**  
AWS Single Sign\-On is a cloud\-based service that simplifies managing AWS SSO access to AWS accounts and business applications\. For more information, see [AWS Single Sign\-On User Guide](http://docs.aws.amazon.com/singlesignon/latest/userguide/)\. For information about the AWS Single Sign\-On calls logged by CloudTrail, see [Logging AWS SSO API Calls with AWS CloudTrail](http://docs.aws.amazon.com/singlesignon/latest/userguide/logging-using-cloudtrail.html)\.  
Support began 12/7/2017

 **AWS WAF**   
AWS WAF is a web application firewall that lets you monitor the HTTP and HTTPS requests that are forwarded to Amazon CloudFront and lets you control access to your content\. For more information, see the [AWS WAF Developer Guide](http://docs.aws.amazon.com/waf/latest/developerguide/)\. For information about the AWS WAF calls logged by CloudTrail, see [Logging AWS WAF API Calls with AWS CloudTrail](http://docs.aws.amazon.com/waf/latest/developerguide/logging-using-cloudtrail.html)\.   
Support began 04/28/2016

## Storage<a name="cloudtrail-supported-services-storage-and-content-delivery"></a>

 **Amazon Elastic Block Store**   
Amazon Elastic Block Store \(Amazon EBS\) allows you to create persistent storage volumes and attach them to Amazon EC2 instances\. Once attached, you can create a file system on top of these volumes, run a database, or use them in any other way you would use a block device\. For more information, see the [Amazon EC2 User Guide for Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/)\. For information about the Amazon EBS calls logged by CloudTrail, see [Logging API Calls Using AWS CloudTrail](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/using-cloudtrail.html)\.  
Support began 11/13/2013

 **Amazon Elastic File System**   
Amazon Elastic File System \(Amazon EFS\) is a file storage service for Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. You can create and configure file systems quickly and easily with Amazon EFS\. CloudTrail logs all Amazon EFS API actions\. For more information, see the [Amazon Elastic File System User Guide](http://docs.aws.amazon.com/efs/latest/ug/)\. For information about the Amazon EFS calls logged by CloudTrail, see [Logging Amazon EFS API Calls with AWS CloudTrail](http://docs.aws.amazon.com/efs/latest/ug/logging-using-cloudtrail.html)\.  
Support began 06/28/2016

 **Amazon Glacier**   
Amazon Glacier is a storage service optimized for data archiving and backup of infrequently used data\. The service is durable, extremely low\-cost, and includes security features\. For more information, see the [Amazon Glacier Developer Guide](http://docs.aws.amazon.com/amazonglacier/latest/dev/)\. For information about the Amazon Glacier calls logged by CloudTrail, see [Logging Amazon Glacier API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/amazonglacier/latest/dev/audit-logging.html)\.  
Support began 12/11/2014

 **Amazon Simple Storage Service**   
You can use Amazon Simple Storage Service \(Amazon S3\) to store and retrieve any amount of data at any time, from anywhere on the web\. You can also use CloudTrail logs together with Amazon S3 server access logs\. For more information, see the [Amazon Simple Storage Service Developer Guide](http://docs.aws.amazon.com/AmazonS3/latest/dev/)\.  
CloudTrail logs Amazon S3 bucket level events such as the creating and deleting buckets, changes to bucket policy, and changes to replication status\. You can also configure your trail to log object level events such as creating, updating, or deleting S3 objects in a bucket\. For more information, see [Logging Amazon S3 API Calls By Using AWS CloudTrail](http://docs.aws.amazon.com/AmazonS3/latest/dev/cloudtrail-logging.html)\.  
Support began 09/01/2015 for management events  
Support began 11/21/2016 for data events

 **AWS Storage Gateway**   
AWS Storage Gateway is a service that connects an on\-premises software appliance with cloud\-based storage to provide seamless and secure integration between your on\-premises IT environment and the AWS storage infrastructure in the cloud\. For more information, see the [AWS Storage Gateway User Guide](http://docs.aws.amazon.com/storagegateway/latest/userguide/)\. For information about the AWS Storage Gateway Volume Gateway calls logged by CloudTrail, see [Logging AWS Storage Gateway API Calls by Using AWS CloudTrail](http://docs.aws.amazon.com/storagegateway/latest/userguide/logging-using-cloudtrail.html)\.  
Support began 12/16/2014

## Support<a name="cloud-trail-supported-services-aws-support"></a>

 **AWS Personal Health Dashboard**   
AWS Health provides ongoing visibility into the state of your AWS resources, services, and accounts\. The service gives you awareness and remediation guidance for resource performance or availability issues that may affect your applications that run on AWS\. CloudTrail logs all AWS Health API operations\. For more information, see the [AWS Health User Guide](http://docs.aws.amazon.com/health/latest/ug/)\. For information about AWS Health API calls logged by CloudTrail, see [Logging AWS Health API Calls with AWS CloudTrail](http://docs.aws.amazon.com/health/latest/ug/logging-using-cloudtrail.html)\.  
Support began 12/01/2016

 **AWS Support**   
AWS Support offers a range of plans that provide access to tools and expertise that support the success and operational health of your AWS solutions\. All support plans provide 24x7 access to customer service, AWS documentation, whitepapers, and support forums\. For more information, see the [AWS Support User Guide](http://docs.aws.amazon.com/awssupport/latest/user/)\. For information about the AWS Support API calls logged by CloudTrail, see [Logging AWS Support API Calls with AWS CloudTrail](http://docs.aws.amazon.com/awssupport/latest/user/logging-using-cloudtrail.html)\.  
Support began 04/21/2016

## Services That Support Logging Data Events<a name="cloud-trail-supported-services-data-events"></a>

Data events provide insight into the resource operations performed on or within a resource\. These are also known as data plane operations\. Data events are often high\-volume activities\. Data events are disabled by default when you create a trail\. To record CloudTrail data events, you must explicitly add the supported resources or resource types for which you want to collect activity to a trail\. For more information, see [Data Events](logging-management-and-data-events-with-cloudtrail.md#logging-data-events)\.

 **Amazon Simple Storage Service \(Amazon S3\)**   
Amazon Simple Storage Service \(Amazon S3\) is storage for the internet\. It is designed to make web\-scale computing easier for developers\.  
You can configure a trail to log Amazon S3 object\-level events such as creating, updating, or deleting S3 objects in a bucket\. For a list of supported data events that CloudTrail logs for Amazon S3 objects, see [Amazon S3 Object\-Level Actions Tracked by CloudTrail Logging](http://docs.aws.amazon.com/AmazonS3/latest/dev/cloudtrail-logging.html#cloudtrail-object-level-tracking) in the *Amazon Simple Storage Service Developer Guide*\.   
Support began 11/21/2016 

 **AWS Lambda**   
AWS Lambda is a zero\-administration compute platform that runs your code in the AWS Cloud, providing the high availability, security, performance, and scalability of AWS infrastructure\. For more information, see the [AWS Lambda Developer Guide](http://docs.aws.amazon.com/lambda/latest/dg/)\. You can configure a trail to log data events for the `Invoke` API\.   
Support began 11/30/2017 