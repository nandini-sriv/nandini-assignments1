2.14 
1. Does SNS guarantee exactly once delivery to subscribers?
No, Amazon Simple Notification Service (SNS) does not guarantee exactly once delivery to subscribers. 
SNS provides at least once delivery, meaning that messages will be delivered to subscribers at least once, 
but there is a possibility that messages could be delivered more than once due to network issues, retries, 
or other temporary failures. If you need exactly once delivery, you would need to implement deduplication logic 
in your subscriber application or use other services, like Amazon SQS with message deduplication.

2. What is the purpose of the Dead-letter Queue (DLQ)?
A Dead-letter Queue (DLQ) is used to store messages that could not be successfully processed by the consumer 
(e.g., a subscriber or another service). It serves as a failover queue where problematic messages are isolated so 
they can be analyzed or retried without affecting the normal message processing flow. DLQs are useful in the 
following scenarios:

Message processing failures: If a message cannot be processed successfully after multiple retries, it is sent to the 
DLQ for later inspection.
Debugging and auditing: The DLQ allows you to inspect failed messages to troubleshoot why they failed.
Preventing message loss: DLQs ensure that no messages are lost, even if they cannot be processed successfully by the 
primary system.
This feature is available in services like SQS, SNS, and EventBridge to ensure that unprocessed messages can be handled 
or retried later.

3. How would you enable a notification to your email when messages are added to the DLQ?
To receive notifications when messages are added to a Dead-letter Queue (DLQ), you can configure an Amazon SNS topic 
and subscribe your email address to that topic. Here’s a step-by-step approach to set it up:

Create an SNS Topic:

Go to the SNS console in AWS.
Click on Create topic.
Choose the type of topic (Standard or FIFO) and give it a name.
Click Create topic.
Subscribe your email to the SNS Topic:

Once the SNS topic is created, click on the topic.
Under the Subscriptions tab, click Create subscription.
Select Email as the protocol.
Enter your email address.
Click Create subscription.
AWS will send a confirmation email to the provided address. Confirm the subscription by clicking the link in the email.
Configure DLQ to Send Notifications to SNS:

For SQS DLQ: When configuring the DLQ for an SQS queue, you can set up an SNS topic to notify you when messages are 
added to the DLQ.
Go to the SQS console.
Select your DLQ.
Under the Dead-letter queue section, find the SNS topic field and select the SNS topic you created earlier.
After this, any message that gets sent to the DLQ will trigger an SNS notification to your email.
For SNS DLQ: SNS itself doesn't have a direct DLQ, but if you're using SNS to send messages to an SQS queue, you can 
configure the SQS queue with a DLQ and follow the steps above to subscribe to SNS for DLQ notifications.
Enable Notification for EventBridge DLQ (if using EventBridge):

In the EventBridge console, you can configure EventBridge rules with a DLQ.
You can then subscribe the SNS topic to the DLQ and configure email notifications, as described above for SQS.
Conclusion:
To summarize, SNS doesn't guarantee exactly once delivery; it provides at least once delivery. The Dead-letter Queue 
(DLQ) is used to isolate and store messages that couldn't be processed successfully, allowing for later analysis or 
retries. To receive notifications when messages are added to the DLQ, you can create an SNS topic, subscribe your email 
to it, and configure the relevant service (e.g., SQS, EventBridge) to send notifications to the SNS topic.
