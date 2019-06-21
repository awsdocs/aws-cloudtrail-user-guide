# Limits in AWS CloudTrail<a name="WhatIsCloudTrail-Limits"></a>

The following table describes limits within CloudTrail\. CloudTrail has no adjustable limits\. For information about other limits in AWS, see [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)\.


| Resource | Default Limit | Comments | 
| --- | --- | --- | 
| Trails per region | 5 | This limit cannot be increased\. | 
| Get, describe, and list APIs | 10 transactions per second \(TPS\) | The maximum number of operation requests you can make per second without being throttled\. The LookupEvents API is not included in this category\.This limit cannot be increased\. | 
| LookupEvents API | 2 transactions per second \(TPS\) | The maximum number of operation requests you can make per second without being throttled\. This limit cannot be increased\. | 
| All other APIs | 1 transaction per second \(TPS\) | The maximum number of operation requests you can make per second without being throttled\. This limit cannot be increased\. | 
| Event selectors | 5 per trail | This limit cannot be increased\. | 
| Data resources in event selectors | 250 across all event selectors in a trail | The total number of data resources cannot exceed 250 across all event selectors in a trail\. The limit of number of resources on an individual event selector is configurable up to 250\. This upper limit is allowed only if the total number of data resources does not exceed 250 across all event selectors\. Examples: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/WhatIsCloudTrail-Limits.html)This limit cannot be increased\. | 