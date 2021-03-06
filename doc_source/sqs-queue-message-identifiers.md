# Queue and Message Identifiers<a name="sqs-queue-message-identifiers"></a>

## General Identifiers<a name="sqs-general-identifiers"></a>

### Queue Name and URL<a name="queue-name-url"></a>

When you create a new queue, you must specify a queue name that is unique within the scope of all your queues\. Amazon SQS assigns each queue you create an identifier called a *queue URL* that includes the queue name and other Amazon SQS components\. Whenever you want to perform an action on a queue, you provide its queue URL\.

The name of a FIFO queue must end with the `.fifo` suffix\. The suffix counts towards the 80\-character queue name limit\. To determine whether a queue is [FIFO](FIFO-queues.md), you can check whether the queue name ends with the suffix\.

The following is the queue URL for a queue named `MyQueue` owned by a user with the AWS account number `123456789012`\.

```
https://sqs.us-east-2.amazonaws.com/123456789012/MyQueue
```

**Important**  
In your system, always store the entire queue URL exactly as Amazon SQS returns it to you when you create the queue \(for example, `https://sqs.us-east-2.amazonaws.com/123456789012/MyQueue`\)\. Don't build the queue URL from its separate components each time you need to specify the queue URL in a request because Amazon SQS can change the components that make up the queue URL\.

You can also get the queue URL for a queue by listing your queues\. For more information, see `[ListQueues](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ListQueues.html)`\.

### Message ID<a name="message-id"></a>

Each message receives a system\-assigned *message ID* that Amazon SQS returns to you in the `[SendMessage](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_SendMessage.html)` response\. This identifier is useful for identifying messages\. \(However, to delete a message you need the message's *receipt handle*\.\) The maximum length of a message ID is 100 characters\.

### Receipt Handle<a name="receipt-handle"></a>

Every time you receive a message from a queue, you receive a *receipt handle* for that message\. This handle is associated with the action of receiving the message, not with the message itself\. To delete the message or to change the message visibility, you must provide the receipt handle \(not the message ID\)\. Thus, you must always receive a message before you can delete it \(you can't put a message into the queue and then recall it\)\. The maximum length of a receipt handle is 1024 characters\. 

**Important**  
If you receive a message more than once, each time you receive it, you get a different receipt handle\. You must provide the most recently received receipt handle when you request to delete the message \(otherwise, the message might not be deleted\)\. 

The following is an example of a receipt handle \(broken across three lines\)\.

```
MbZj6wDWli+JvwwJaBV+3dcjk2YW2vA3+STFFljTM8tJJg6HRG6PYSasuWXPJB+Cw
Lj1FjgXUv1uSj1gUPAWV66FU/WeR4mq2OKpEGYWbnLmpRCJVAyeMjeU5ZBdtcQ+QE
auMZc8ZRv37sIW2iJKq3M9MFx1YvV11A2x/KSbkJ0=
```

## Additional Identifiers for FIFO Queues<a name="sqs-additional-fifo-queue-identifiers"></a>

For more information about the following identifiers, see [Exactly\-Once Processing](FIFO-queues.md#FIFO-queues-exactly-once-processing) and the *[Amazon Simple Queue Service API Reference](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/)*\.

### Message Deduplication ID<a name="sqs-fifo-queue-messagededuplicationid"></a>

The token used for deduplication of sent messages\. If a message with a particular message deduplication ID is sent successfully, any messages sent with the same message deduplication ID are accepted successfully but aren't delivered during the 5\-minute deduplication interval\.

### Message Group ID<a name="sqs-fifo-queue-messagegroupid"></a>

The tag that specifies that a message belongs to a specific message group\. Messages that belong to the same message group are always processed one by one, in a strict order relative to the message group \(however, messages that belong to different message groups might be processed out of order\)\.

### Sequence Number<a name="sqs-fifo-queue-sequence-number"></a>

The large, non\-consecutive number that Amazon SQS assigns to each message\.