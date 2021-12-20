# Chapter 11- Stream Processing
In batch processing data is bounded, but real data is mostly unbounded, users will continue to buy products tomorrow and the day after. 
Most bounds on data are arbitrary. Stream processing deals with unbounded data.
Problem with batch processing is that changes are reflected only a day later.
What if we want changes instantaneously after one record changes?

## Examples
stdin, stdout of Unix, programming languages (lazy lists), file system APIS(Java FileInputStream), TCP connections, delivering audio and video over internet.

## Event streams
In batch processing, input and output are files, file are parsed into records in streams, records are known as events. 
We have producers that generate data and consumers that consume that data. The so called data or events might be user activity, temp measurement sensor.

## Polling
Producer writes event to the datastore and consumer periodically checks for the event. Polling can be expensive for low delays. Better way is to notify consumers when events happen so it can read data.
Publish/Subscribe is another method to distribute information to consumers, the idea is that some consumers subscribe to messagesfrom the consumer. The following are important considerations to take in this case:

## What happens if the producers produce messages faster than they can be consumer?
1. Queue it in memory.
2. Drop messages
3. backpressure (flow control) used by TCP and Unix pipes
What happens if nodes crash or go offline?
Same reliability considerations that we take with databases must be taken here.

In some apps lost messages can be ignored like in sensor readings.
Batch processing is good in the sense that we can simply restart after a shutdown.

Producer consumer communication using Webhooks
Consumer expose endpoints where producers can transmit some events we register for.

## Producer consumer communication using message brokers (message queue)
- A database optimized for message streams.
- Acts as a server to which producers and consumers connect to as clients.
- Producers send messages to queue and consumers consume message.
- By centralizing the data in the queue, the question of durabilityi is moved to the queue.
- Some brokers keep message in memory while some keep it in storage in case system crashes.
- Quing is async, producer only waits for message to be put in queue buffer and not that it has been sent to consumer.

## Databases vs Message brokers(queues)
- Databases keep data until it is deleted, brokers delete message after it has been deleted.
- Since they are sent as soon as they are deleted, most message queues assume they have small data and thus can have slow throughput in case of slow consumers.
- Databases have secondary indexes while message queues have a way to subscribe to messages with a specific pattern.
- Databases have queuing services and message queues notify consumers when data changes.

This was a traditional message queue which is encapsulated in standards such as JMS and AMQP and implemented in software like RabbitMQ,
ActiveMQHornetQ, Qpid, TIBCO Enterprise Message Service, IBM MQ, Azure Service
Bus, and Google Cloud Pub/Sub.

## What if message queue fails? Acknowledgments and redelivery
Consumers may fail at any time so we it's possible after receiving a message from producer a consumer crashes mid processing,
In order to ensure that the message is not lost, message brokers use acknowledgments: a client must explicitly tell the broker when 
it has finished processing a message so that the broker can remove it from the queue.

## Partioned logs/ Using logs as message queuing
Traditional JSM standard message queues do not save any information about the event being transmitted after it has been sent. 
If you add a consumer to a message queue later, it cannot read previous records.
On the contrary, databases log everything unless explicitly deleted. Using logs as message queues combines this.
Since logs are appending only, producers append data to logs and consumers read logs. these logs can be portioned to use more machines.
And replicated. Each portioned is ordered since logs are appending only. Consumers read sequentially.

## Examples
Apache Kafka, Amazon Kinesis Streams, and Twitter’s DistributedLog.
Google Cloud Pub/Sub is architecturally similar but exposes a JMS-style API rather than a log abstraction.

In a situation where order of messages does not matter, paralization is important and message processing is important the JSM/AMQP style 
of message brokers are preferable. When each message is fast to process and order is important log based message brokers are preferred.

## Change Data Capture
In a system with a database for OLTP, a cache for common requests, a search index or searching and streams for data propagation we
need a way to propagate a way to map changes from source of truth(database) to these storage systems. One way to do this is to code it in application logic but that can cause race conditions. A newer approach is to directly read the logs the database creates.
 This is CDC. We can use database triggers to make a call to an event stream that updates all these storage systems.
## Event sourcing
Capturing application level logs. It is similar to change data capture but unlike change data capture. It records only application level
logs and not granular database logs. It is great for debugging and going back in case of a transaction.

## Processing Streams

### We have three options
1. Process the stream and put it in a database, search index or chache.
2. Use the stream as a push notification to customers as a dashboard or message notification.
3. Use stream results to map it into something to pass it into another stream pipeline.

## Use of stream processing
### Examples
- Fraud detection.
- Trade events.
- Monitor status of machinery.
- Military use it to track the activity of an aggressor.

## Complex event processing
Geared towards searching a specific pattern like a regular expression searches a string. It searches continuously for a matching pattern.
### Examples
Esper , IBM InfoSphere Streams, Apama,TIBCO StreamBase, and SQLstream.

## Stream analytics
Analytics tends to be less interested in finding specific event sequences and is more oriented toward aggregations and statistical 
metrics over a large number of events.
### Example
1. Measuring the rate of some type of event (how often it occurs per time interval)
2. Calculating the rolling average of a value over some time period
3. Comparing current statistics to previous time intervals












