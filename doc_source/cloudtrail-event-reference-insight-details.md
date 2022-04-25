# CloudTrail Insights `insightDetails` element<a name="cloudtrail-event-reference-insight-details"></a>

AWS CloudTrail Insights event records include fields that are different from other CloudTrail events in their JSON structure, sometimes called *payload*\. A CloudTrail Insights event record includes an `insightDetails` block that contains information about the underlying triggers of an Insights event, such as event source, user identities, user agents, historical averages or *baselines*, statistics, API name, and whether the event is the start or end of the Insights event\. The `insightDetails` block contains the following information\.
+ **`state`** \- Whether the event is the starting or ending Insights event\. The value can be `Start` or `End`\.

  **Since:** 1\.07

  **Optional:** False
+ **`eventSource`** \- The AWS service endpoint that was the source of the unusual activity, such as `ec2.amazonaws.com`\.

  **Since:** 1\.07

  **Optional:** False
+ **`eventName`** \- The name of the Insights event, typically the name of the API that was the source of the unusual activity\.

  **Since:** 1\.07

  **Optional:** False
+ **`insightType`** \- The type of Insights event\. This value can be `ApiCallRateInsight`, `ApiErrorRateInsight`, or both\.

  **Since:** 1\.07

  **Optional:** False
+ **`insightContext`** \- 

  Information about the AWS tools \(called *user agents*\), IAM users and roles \(called *user identities*\), and error codes associated with the events that were analyzed to generate the Insights event\. This element also includes statistics showing how the unusual activity in an Insights event compares to *baseline*, or normal activity\.

  **Since:** 1\.07

  **Optional:** False
  + **`statistics`** \- Includes data about the *baseline*, or typical average rate of calls to or errors on the subject API by an account as measured during the baseline period, the average rate of calls or errors that triggered the Insights event over the first minute of the Insights event, the duration, in minutes, of the Insights event, and the duration, in minutes, of the baseline measuring period\.

    **Since:** 1\.07

    **Optional:** False
    + **`baseline`** \- The average number of API calls or errors per minute during the baseline duration on the Insights event's subject API for the account, calculated over the seven days preceding the start of the Insights event\.

      **Since:** 1\.07

      **Optional:** False
    + **`insight`** \- 

      For a starting Insights event, this value is the average number of API calls or errors per minute during the start of the unusual activity\. For an ending Insights event, this value is the average number of API calls or errors per minute over the duration of the unusual activity\.

      **Since:** 1\.07

      **Optional:** False
    + **`insightDuration`** \- The duration, in minutes, of an Insights event \(the time period from the start to the end of unusual activity on the subject API\)\. `insightDuration` occurs in both starting and ending Insights events\.

      **Since:** 1\.07

      **Optional:** False
    + **`baselineDuration`** \- The duration, in minutes, of the baseline period \(the time period that normal activity is measured on the subject API\)\. `baselineDuration` is at minimum the seven days \(10080 minutes\) preceding an Insights event\. This field occurs in both starting and ending Insights events\. The ending time of `baselineDuration` measurement is always the start of an Insights event\.

      **Since:** 1\.07

      **Optional:** False
  + **`attributions`** \- This block includes information about the user identities, user agents, and error codes correlated with unusual and baseline activity\. A maximum of five user identities, five user agents, and five error codes are captured in an Insights event `attributions` block, sorted by an average of the count of activity, in descending order from highest to lowest\.

    **Since:** 1\.07

    **Optional:** True
    + **`attribute`** \- Contains the attribute type\. Value can be `userIdentityArn`, `userAgent`, or `errorCode`\.
      + **`userIdentityArn`** \- A block that shows up to the top five AWS users or IAM roles that contributed to API calls or errors during the unusual activity and baseline periods\. See also `userIdentity` in [CloudTrail record contents](cloudtrail-event-reference-record-contents.md)\.

        **Since:** 1\.07

        **Optional:** False
        + **`insight`** \- A block that shows up to the top five user identity ARNs that contributed to the API calls made during the unusual activity period, in descending order from largest number of API calls to smallest\. It also shows the average number of API calls made by the user identities during the unusual activity period\.

          **Since:** 1\.07

          **Optional:** False
          + **`value`** \- The ARN of one of the top five user identities that contributed to the API calls made during the unusual activity period\.

            **Since:** 1\.07

            **Optional:** False
          + **`average`** \- The number of API calls or errors per minute during the unusual activity period for the user identity in the `value` field\.

            **Since:** 1\.07

            **Optional:** False
        + **`baseline`** \- A block that shows up to the top five user identity ARNs that contributed the most to the API calls or errors during the normal activity period\. It also shows the average number of API calls or errors logged by the user identities during the normal activity period\.

          **Since:** 1\.07

          **Optional:** False
          + **`value`** \- The ARN of one of the top five user identities that contributed to the API calls or errors during the normal activity period\.

            **Since:** 1\.07

            **Optional:** False
          + **`average`** \- The historic average of API calls or errors per minute during the seven days preceding the Insights activity start time for the user identity in the `value` field\.

            **Since:** 1\.07

            **Optional:** False
      + **`userAgent`** \- A block that shows up to the top ﬁve AWS tools by which the user identity contributed to API calls during the unusual activity and baseline periods\. These tools include the AWS Management Console, AWS CLI, or the AWS SDKs\. See also `userAgent` in [CloudTrail record contents](cloudtrail-event-reference-record-contents.md)\.

        **Since:** 1\.07

        **Optional:** False
        + **`insight`** \- A block that shows up to the top five user agents that contributed to the API calls made during the unusual activity period, in descending order from largest number of API calls to smallest\. It also shows the average number of API calls or errors logged by the user agents during the unusual activity period\.

          **Since:** 1\.07

          **Optional:** False
          + **`value`** \- One of the top five user agents that contributed to the API calls made during the unusual activity period\.

            **Since:** 1\.07

            **Optional:** False
          + **`average`** \- The number of API calls or errors logged per minute during the unusual activity period for the user agent in the `value` field\.

            **Since:** 1\.07

            **Optional:** False
        + **`baseline`** \- A block that shows up to the top five user agents that contributed the most to the API calls made during the normal activity period\. It also shows the average number of API calls or errors logged by the user agents during the normal activity period\.

          **Since:** 1\.07

          **Optional:** False
          + **`value`** \- One of the top five user agents that contributed to the API calls or errors logged during the normal activity period\.

            **Since:** 1\.07

            **Optional:** False
          + **`average`** \- The historic average of API calls or errors per minute during the seven days preceding the Insights activity start time for the user agent in the `value` field\.

            **Since:** 1\.07

            **Optional:** False
      + **`errorCode`** \- A block that shows up to the top five error codes that occurred on API calls during the unusual activity and baseline periods, in descending order from largest number of API calls to smallest\. See also `errorCode` in [CloudTrail record contents](cloudtrail-event-reference-record-contents.md)\.

        **Since:** 1\.07

        **Optional:** False
        + **`insight`** \- A block that shows up to the top five error codes that occurred on the API calls made during the unusual activity period, in descending order from largest number of associated API calls to smallest\. It also shows the average number of API calls on which the errors occurred during the unusual activity period\.

          **Since:** 1\.07

          **Optional:** False
          + **`value`** \- One of the top five error codes that occurred on the API calls made during the unusual activity period, such as `AccessDeniedException`\.

            If none of the calls that triggered the Insights event resulted in errors, this value is `null`\.

            **Since:** 1\.07

            **Optional:** False
          + **`average`** \- The number of API calls per minute during the unusual activity period for the error code in the `value` field\.

            If the error code value is `null`, and there are no other error codes in the `insight` block, the value of the `average` is the same as that in the `statistics` block for the Insights event overall\.

            **Since:** 1\.07

            **Optional:** False
        + **`baseline`** \- A block that shows up to the top five error codes that occurred on the API calls made during the normal activity period\. It also shows the average number of API calls made by the user agents during the normal activity period\.

          **Since:** 1\.07

          **Optional:** False
          + **`value`** \- One of the top five error codes that occurred on the API calls made during the normal activity period, such as `AccessDeniedException`\.

            **Since:** 1\.07

            **Optional:** False
          + **`average`** \- The historic average of API calls or errors per minute during the seven days preceding the Insights activity start time for the error code in the `value` field\.

            **Since:** 1\.07

            **Optional:** False

## Example `insightDetails` block<a name="event-reference-insight-details-example"></a>

The following is an example of an Insights event `insightDetails` block for an Insights event that occurred when the Application Auto Scaling API `CompleteLifecycleAction` was called an unusual number of times\. For an example of a full Insights event, see [CloudTrail log event reference](cloudtrail-event-reference.md)\.

This example is from a starting Insights event, indicated by `"state": "Start"`\. The top user identities that called the APIs associated with the Insights event, `CodeDeployRole1`, `CodeDeployRole2`, and `CodeDeployRole3`, are shown in the `attributions` block, along with their average API call rates for this Insights event, and the baseline for the `CodeDeployRole1` role\. The `attributions` block also shows that the user agent is `codedeploy.amazonaws.com`, meaning the top user identities used the AWS CodeDeploy console to run the API calls\.

Because there are no error codes associated with the events that were analyzed to generate the Insights event \(the value is `null`\), the `insight` average for the error code is the same as the overall `insight` average for the entire Insights event, shown in the `statistics` block\.

```
          "insightDetails": {
            "state": "Start",
            "eventSource": "autoscaling.amazonaws.com",
            "eventName": "CompleteLifecycleAction",
            "insightType": "ApiCallRateInsight",
            "insightContext": {
              "statistics": {
                "baseline": {
                  "average": 0.0000882145
                },
                "insight": {
                  "average": 0.6
                },
                "insightDuration": 5,
                "baselineDuration": 11336
              },
              "attributions": [
                {
                  "attribute": "userIdentityArn",
                  "insight": [
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole1",
                      "average": 0.2
                    },
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole2",
                      "average": 0.2
                    },
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole3",
                      "average": 0.2
                    }
                  ],
                  "baseline": [
                    {
                      "value": "arn:aws:sts::012345678901:assumed-role/CodeDeployRole1",
                      "average": 0.0000882145
                    }
                  ]
                },
                {
                  "attribute": "userAgent",
                  "insight": [
                    {
                      "value": "codedeploy.amazonaws.com",
                      "average": 0.6
                    }
                  ],
                  "baseline": [
                    {
                      "value": "codedeploy.amazonaws.com",
                      "average": 0.0000882145
                    }
                  ]
                },
                {
                  "attribute": "errorCode",
                  "insight": [
                    {
                      "value": "null",
                      "average": 0.6
                    }
                  ],
                  "baseline": [
                    {
                      "value": "null",
                      "average": 0.0000882145
                    }
                  ]
                }
              ]
            }
          }
```