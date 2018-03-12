# Stopping CloudTrail from Sending Events to CloudWatch Logs<a name="stop-cloudtrail-from-sending-events-to-cloudwatch-logs"></a>

You can stop sending events to CloudWatch Logs by deleting the delivery endpoint\.

## AWS Management Console<a name="stop-cloudtrail-from-sending-events-to-cloudwatch-logs-console"></a>

**To remove the CloudWatch Logs delivery endpoint using the AWS Management Console**

1. Sign in to the AWS Management Console\.

1. Navigate to the CloudTrail console\.

1. In the navigation pane, click **Configuration**\.

1. In the **CloudWatch Logs \(optional\)** section, click the **Delete** \(trash can\) icon\.

1. Click **Continue** to confirm\.

## AWS Command Line Interface \(CLI\)<a name="stop-cloudtrail-from-sending-events-to-cloudwatch-logs-cli"></a>

You can remove the CloudWatch Logs log group as a delivery endpoint using the `update-trail` command\. The following command clears the log group and role from the trail configuration\.

```
aws cloudtrail update-trail --name trailname --cloud-watch-logs-log-group-arn="" --cloud-watch-logs-role-arn=""
```