---
title: "Pub-Sub vs Message Queue vs Message Broker"
source: "https://blog.algomaster.io/p/pub-sub-vs-message-queue-vs-message-broker"
tags:
  - "source/article"
  - "status/todo"
author:
  - "Ashish Pratap Singh"
cover: "https://substackcdn.com/image/fetch/$s_!kC8x!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48800e6c-b0e5-4fe0-a9e4-7f667b1e66a7_1830x1388.png"
words: 1067
---
### What’s the difference?

When you’re working on a distributed system and need components to **communicate asynchronously**, you’ll hear terms like: **“pub-sub”, “message queue”**, and **“message broker”**.

Although they are used interchangeably sometimes, they’re not the same thing.

**Simply put:**

> A message broker is the **infrastructure**. Message queues and pub-sub are **messaging patterns** that brokers implement.

In this article, we’ll explore:

- What each term means
- How they differ architecturally
- When to use each pattern
- Real-world examples and use cases
- How modern systems like Kafka, RabbitMQ, and Redis fit in

---

## 1\. The Core Concepts

Before diving into comparisons, let’s define each term clearly.

## 1.1 Message Broker

A **message broker** is the middleware infrastructure that receives, stores, and routes messages between producers and consumers. It’s the “thing” that sits in the middle.

![](https://substackcdn.com/image/fetch/$s_!VjPv!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F174a1db8-0687-48b2-b2f7-e9e37a3ec1c6_1962x548.png)

Think of a message broker like a post office. It receives mail (messages), stores it temporarily, and delivers it to the right recipients based on addresses (routing rules).

**Examples:** RabbitMQ, Apache Kafka, ActiveMQ

#### Key responsibilities:

- Accept messages from producers
- Store messages durably or transiently
- Route messages to appropriate consumers
- Handle delivery guarantees (at-least-once, exactly-once)
- Manage consumer acknowledgments

## 1.2 Message Queue

A **message queue** is a messaging pattern where messages are sent to a queue and consumed by exactly one consumer. Messages are typically processed in order and removed from the queue after consumption.

![](https://substackcdn.com/image/fetch/$s_!SGXq!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2726eea3-4c83-4894-8c3e-f1788fc9841e_1902x588.png)

Message queues enable **asynchronous service-to-service communication**, especially in serverless and microservices architectures. They allow you to build reliable background processing, work distribution, and decoupled systems.

**Examples:** SQS queues, RabbitMQ queues

When multiple consumers connect to the same queue, they compete for messages. Each message goes to exactly one consumer.

![](https://substackcdn.com/image/fetch/$s_!QzR0!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed34852e-2684-4403-b2b7-7270f92065ca_1810x904.png)

#### Key Characteristics:

- **Delivery:** Exactly one consumer receives each message
- **Ordering:** First-In-First-Out (FIFO) ordering within a single queue
- **Persistence:** Messages stored until consumed and acknowledged
- **Acknowledgment:** Consumer must confirm successful processing
- **Retry:** Failed messages can be retried or dead-lettered

## 1.3 Pub-Sub (Publish-Subscribe)

**Pub-Sub** is a messaging pattern where publishers send messages to topics, and all subscribers to that topic receive a copy of the message. Messages are broadcast, not consumed exclusively.

![](https://substackcdn.com/image/fetch/$s_!ZveM!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9ef3ab1f-d1a7-4cfc-909b-5869cadc3265_1988x836.png)

Think of it like a radio broadcast. The station (publisher) broadcasts a signal (message), and everyone tuned to that frequency (subscribers) receives it simultaneously.

It’s an **asynchronous communication** model that decouples services, making it easier to build **scalable, event-driven systems**. Pub/Sub is widely used in modern cloud architectures to deliver instant event notifications and enable reliable communication between independent components.

**Examples:** Redis Pub/Sub, SNS

Pub-sub naturally implements fan-out in event-drive architectures, where one event triggers multiple actions:

![](https://substackcdn.com/image/fetch/$s_!ZdUz!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2056c392-d7ce-4dec-9945-3dcc47baf27f_1852x758.png)

#### Key Characteristics:

- **Delivery:** One-to-many. Each message delivered to all subscribers.
- **Decoupling:** Publishers and subscribers are decoupled
- **Scalability:** Easy to add new subscribers
- **Persistence:** Messages may or may not be removed after delivery (depends on implementation)

---

## 2\. How Brokers Implement Both

Modern message brokers typically support both patterns. Here’s how popular brokers handle each:

## 2.1 RabbitMQ

RabbitMQ uses **exchanges** to support both patterns:

- **Direct Exchange (Message Queue style):** Routes messages to queues based on an exact routing key. Great for work distribution to specific consumers.
- **Fanout Exchange (Pub-Sub style):** Broadcasts every message to all bound queues. Ideal for event broadcasting to multiple consumers.

![](https://substackcdn.com/image/fetch/$s_!kC8x!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F48800e6c-b0e5-4fe0-a9e4-7f667b1e66a7_1830x1388.png)

## 2.2 Apache Kafka

Kafka’s architecture is built around an **append-only log**, and it supports both patterns using **consumer groups**.

#### Message Queue in Kafka (competing consumers):

![](https://substackcdn.com/image/fetch/$s_!2m1o!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93fd1937-8b36-4476-ae3a-77e9b4a445fc_1232x966.png)

- A topic is split into **partitions**.
- In a consumer group, **each partition is consumed by exactly one consumer**.

#### Pub-Sub in Kafka (independent consumers):

![](https://substackcdn.com/image/fetch/$s_!F2Ah!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38dc7ed2-6c0a-4e00-8a6f-93b795358a35_1516x742.png)

- **Each consumer group gets all messages** from the topic.
- You can have many groups (analytics, notifications, billing, etc.), all reading the same stream independently.
- Within each group, messages are still load-balanced across consumers like a queue.

## 2.3 Amazon Services

AWS offers separate services for each pattern.

- **Simple Queue Service (SQS)** for Message Queues
- **Simple Notification Service (SNS)** for Pub-Sub

A common pattern is to combine both SNS and SQS in an even-driver architecture:

![](https://substackcdn.com/image/fetch/$s_!zB_O!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdcf8ed03-aeb8-4f36-a098-fa156315fbf7_1742x466.png)

- Use **SNS** for **fan-out** (publish an event once, deliver to many targets).
- Attach **SQS queues** as subscribers for **durable, reliable delivery** to each downstream service.

This gives you pub-sub style broadcasting with queue-style reliability and independent scaling for each consumer.

## 2.4 Redis

Redis supports both patterns using different data structures and commands.

Lists give you queue-like behavior with persistence in memory, while Pub/Sub is ephemeral and only delivers messages to clients that are currently subscribed.

#### Lists (Message Queue):

Use a list as a simple queue: producers push, consumers block and pop.

```markup
LPUSH myqueue “message” --> Publisher sends a message

BRPOP myqueue 0         --> Consumer blocks until a message is available
```

#### Pub/Sub (Broadcast):

Use Redis Pub/Sub to broadcast messages to all active subscribers.

```markup
PUBLISH channel “message”  --> Publisher sends a message

SUBSCRIBE channel          --> Subscriber listens for messages
```

---

## 3\. Choosing the Right Pattern

A **message queue** is about **work**. Each message represents a unit of work that should be done **once** by **one** worker.

**Pub-Sub** is about **events**. Something happened, and many services might care.

#### Use a Message Queue when you need:

- **Task processing:** Each task must be executed exactly once.
- **Work distribution:** You want to spread load evenly across multiple workers.
- **Background jobs:** Long-running or async tasks with reliable delivery.
- **Rate limiting:** You need to control how fast consumers process messages.
- **Sequential processing:** FIFO ordering for messages in a queue.

#### Use Pub-Sub when you need:

- **Event notifications:**Multiple services should react to the same event.
- **Real-time updates:** Broadcast changes (e.g., price updates, notifications) to many clients.
- **Decoupled microservices:** Publishers and subscribers can evolve independently without tight coupling.

#### In short:

- If every message is “do this work once,” you’re in message queue territory.
- If every message is “this happened, whoever cares may react,” you’re in pub-sub territory.

---

## Summary

Message queues and pub-sub are **patterns**, not mutually exclusive tools. A message broker is the infrastructure that often supports both.

- Need **one consumer per message**? → Use a **Message Queue**.
- Need **every subscriber to get every message**? → Use **Pub-Sub**.

Most real-world systems combine them:

- **Pub-Sub** for cross-service events.
- **Queues** for work distribution within or behind services.

Modern brokers (Kafka, RabbitMQ, cloud messaging services) usually support both patterns. The key is to choose based on **delivery semantics** and **durability** requirements, not on tool names alone.

---

Thank you for reading!

If you found it valuable, hit a like ❤️ and consider subscribing for more such content.

---

**P.S.** If you’re enjoying this newsletter and want to get even more value, consider becoming a **[paid subscriber](https://blog.algomaster.io/subscribe)**.

As a paid subscriber, you’ll unlock all **premium articles** and gain full access to all **[premium courses](https://algomaster.io/newsletter/paid/resources)** on **[algomaster.io](https://algomaster.io/)**.

![](https://substackcdn.com/image/fetch/$s_!hE7B!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F89dbd913-ed77-4eee-a193-e7e9b494b891_1212x1007.png)

**There are [group discounts](https://blog.algomaster.io/subscribe?group=true), [gift options](https://blog.algomaster.io/subscribe?gift=true), and [referral bonuses](https://blog.algomaster.io/leaderboard) available.**

---

Checkout my **[Youtube channel](https://www.youtube.com/@ashishps_1/videos)** for more in-depth content.

Follow me on **[LinkedIn](https://www.linkedin.com/in/ashishps1/)**, **[X](https://twitter.com/ashishps_1)** and **[Medium](https://medium.com/@ashishps)** to stay updated.

Checkout my **[GitHub repositories](https://github.com/ashishps1)** for free interview preparation resources.

I hope you have a lovely day!

See you soon,  
Ashish