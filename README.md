# Assignment-2.14
Serverless Architecture II

# AWS Messaging Concepts

## Does SNS guarantee exactly once delivery to subscribers?

No, Amazon SNS provides **at-least-once** message delivery to subscribers. This means that a message might be delivered more than once, but it will be delivered at least once. Subscribers need to implement idempotency to handle potential duplicate messages.

## What is the purpose of the Dead-letter Queue (DLQ)? This is a feature available to SQS/SNS/EventBridge.

The purpose of a Dead-letter Queue (DLQ) is to serve as a holding place for messages that cannot be successfully processed by the consumer. This can happen for various reasons, such as:

* The consumer application is experiencing errors.
* The message format is invalid.
* The consumer is unable to process the message within a specified number of retries.

By moving these problematic messages to a DLQ, you can:

* Prevent them from repeatedly being attempted and potentially causing further issues.
* Analyze the messages in the DLQ to understand why they failed and troubleshoot the problem.
* Reprocess the messages manually or after fixing the underlying issue, if necessary.

## How would you enable a notification to your email when messages are added to the DLQ?

You can enable email notifications for messages added to a DLQ using the following steps, typically involving AWS services:

1.  **Create an SNS Topic:** Create a new SNS topic that will be used to send the email notifications.
2.  **Subscribe your Email to the SNS Topic:** Add your email address as a subscriber to the SNS topic. You will need to confirm the subscription via an email sent by AWS.
3.  **Configure CloudWatch Alarm for the DLQ:**
    * Create a CloudWatch metric for the `ApproximateNumberOfMessagesVisible` metric of your DLQ.
    * Create a CloudWatch Alarm based on this metric, triggering when the number of messages is greater than 0.
    * Set the alarm's action to send a notification to the SNS topic.

When a message ends up in the DLQ, the CloudWatch alarm will trigger, sending an email notification.
