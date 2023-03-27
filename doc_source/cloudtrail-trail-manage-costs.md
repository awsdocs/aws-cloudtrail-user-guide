# Managing CloudTrail costs<a name="cloudtrail-trail-manage-costs"></a>

As a best practice, we recommend using AWS services and tools that can help you manage CloudTrail costs\. You can also configure and manage CloudTrail trails and event data stores in ways that capture the data you need while remaining cost\-effective\. For more information about CloudTrail pricing, see [AWS CloudTrail Pricing](http://aws.amazon.com/cloudtrail/pricing/)\.

## Tools to help manage costs<a name="cloudtrail-trail-manage-costs-tools"></a>

AWS Budgets, a feature of AWS Billing and Cost Management, lets you set custom budgets that alert you when your costs or usage exceed \(or are forecasted to exceed\) your budgeted amount\.

As you create multiple trails or event data stores, creating a budget for CloudTrail by using AWS Budgets is a recommended best practice, and can help you track your CloudTrail spending\. Cost\-based budgets help promote awareness of how much you might be billed for your CloudTrail use\. [Budget alerts](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-best-practices.html#budgets-best-practices-alerts) notify you when your bill reaches a threshold that you define\. When you receive a budget alert, you can make changes before the end of the billing cycle to manage your costs\.

After you [create a budget](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-create.html), you can use AWS Cost Explorer to see how your CloudTrail costs are influencing your overall AWS bill\. In AWS Cost Explorer, after adding CloudTrail to the **Service** filter, you can compare your historical CloudTrail spending to that of your current month\-to\-date \(MTD\) spending, by both region and account\. This feature helps you monitor and detect unexpected costs in your monthly CloudTrail spending\. Additional features in Cost Explorer let you compare CloudTrail spending to monthly spending at the specific resource level, providing information about what might be driving cost increases or decreases in your bill\.

**Note**  
Though you can apply tags to CloudTrail trails, AWS Billing cannot currently use tags applied to trails for cost allocation\. Cost Explorer can show costs for CloudTrail Lake event data stores and for the CloudTrail service as a whole\.

To get started with AWS Budgets, open [AWS Billing and Cost Management](https://console.aws.amazon.com/billing), and then choose **Budgets** in the left navigation bar\. We recommend configuring budget alerts as you create a budget to track CloudTrail spending\. For more information about how to use AWS Budgets, see [Managing Your Costs with Budgets](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-managing-costs.html) and [Best Practices for AWS Budgets](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-best-practices.html)\.

### Creating user\-defined cost allocation tags for CloudTrail Lake event data stores<a name="cloudtrail-trail-manage-costs-tags"></a>

You can create [user\-defined cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/custom-tags.html) to track the query and ingestion costs for your CloudTrail Lake event data stores\. A *user\-defined cost allocation tag* is a key\-value pair that you can associate with an event data store\. After you activate cost allocation tags, AWS uses the tags to organize your resource costs on your cost allocation report\.
+ To create tags in the console, see step 7 of the [To create an event data store for CloudTrail events](query-event-data-store-cloudtrail.md#query-event-data-store-cloudtrail-procedure) procedure\.
+ To create tags using the CloudTrail API, see [CreateEventDataStore](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_CreateEventDataStore.html) and [AddTags](https://docs.aws.amazon.com/awscloudtrail/latest/APIReference/API_AddTags.html) in the *AWS CloudTrail API Reference*\. You can also define 
+ To create tags using the AWS CLI, see [create\-event\-data\-store](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/create-event-data-store.html) and [add\-tags](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/cloudtrail/add-tags.html) in the *AWS CLI Command Reference*\.

For more information about activating tags, see [Activating user\-defined cost allocation tags](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/activating-tags.html)\.

## Trail configuration<a name="cloudtrail-trail-manage-costs-trailconfig"></a>

CloudTrail offers flexibility in how you configure trails in your account\. Some decisions that you make during the setup process require that you understand the impacts to your CloudTrail bill\. The following are examples of how trail configurations can influence your CloudTrail bill\.

**Multiple trail creation**  
The first delivery of each management event for an account is free\. If you create more trails that deliver the same management events to other destinations, those subsequent deliveries incur CloudTrail costs\. You can do this to allow different user groups \(such as developers, security personnel, and IT auditors\) to receive their own copies of log files\. For data events, all deliveries incur CloudTrail costs, including the first\.  
As you create more trails, it is especially important to be familiar with your logs, and understand the types and volumes of events that are generated by resources in your account\. This helps you anticipate the volume of events that are associated with an account, and plan for trail costs\. For example, using AWS KMS\-managed server\-side encryption \(SSE\-KMS\) on your S3 buckets can result in a large number of AWS KMS management events in CloudTrail\. Larger volumes of events across multiple trails can also influence costs\.  
To help limit the number of events that are logged to your trail, you can filter out AWS KMS or Amazon RDS Data API events by choosing **Exclude AWS KMS events** or **Exclude Amazon RDS Data API events** on the **Create trail** or **Update trail** pages\. The option to filter out events is only available if your trail is logging management events\. For more information, see [Creating a trail](cloudtrail-create-a-trail-using-the-console-first-time.md) or [Updating a trail](cloudtrail-update-a-trail-console.md) in this guide\.

**AWS Organizations**  
When you set up an Organizations trail with CloudTrail, CloudTrail replicates the trail to each member account within your organization\. The new trail is created *in addition to* any existing trails in member accounts\. Be sure that the configuration of your organization trail matches how you want trails configured for all accounts within an organization, because the organization trail configuration propagates to all accounts\.  
Because Organizations creates a trail in each member account, an individual member account that creates an additional trail to collect the same management events as the Organizations trail is collecting a second copy of events\. The account is charged for the second copy\. Similarly, if an account has a multi\-region trail, and creates a second trail in a single region to collect the same management events as the multi\-region trail, the trail in the single region is delivering a second copy of events\. The second copy incurs charges\.

## See also<a name="w131aab9c31b7b9"></a>
+ [AWS CloudTrail Pricing](http://aws.amazon.com/cloudtrail/pricing/)
+ [Managing Your Costs with Budgets](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/budgets-managing-costs.html)
+ [Getting Started with Cost Explorer](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/ce-getting-started.html)
+ [Prepare for creating a trail for your organization](creating-an-organizational-trail-prepare.md)