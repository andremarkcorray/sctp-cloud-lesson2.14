# sctp-cloud-lesson2.14
Assignment: Exploring Serverless Architecture 2 - Advanced Concepts and Patterns

1. Does SNS guarantee exactly once delivery to subscribers?
No, SNS does not guarantee exactly-once delivery. It guarantees at least-once delivery to each subscribed endpoint. This means a message may be delivered multiple times to a subscriber in some cases, for e.g. if there was a problem with delivery. Therefore, consumers must be idempotent (able to handle duplicate messages safely).

3. What is the purpose of the Dead-letter Queue (DLQ)?
A Dead-letter Queue (DLQ) captures messages that could not be successfully processed by the target service (eg SQS, SNS, or EventBridge) after a number of delivery attempts. The DLQ can be useful for with:
- Debugging and isolating problematic messages
- Preventing endless retries that can cause delays
- Improving reliability and observability in event-driven systems

3. How would you enable a notification to your email when messages are added to the DLQ?
You can create an EventBridge rule that monitors the DLQ for new messages and triggers a notification via Amazon SNS. Here is the breakdown of steps
a. Create an SNS Topic and subscribe your email address to it
b. Create an EventBridge rule with the following settings:
i. Event source: aws.sqs
ii. Detail type: Match events related to ApproximateNumberOfMessagesVisible
c. Set the rule target as the SNS topic created

This setup allows EventBridge to detect when a message is sent to the DLQ and notify subscriber by email via SNS.
