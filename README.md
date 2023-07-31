# Apache Kafka 

## What is Apache Kafka 
- Apache Kafka is a popular distributed streaming platform that allows you to collect, process, and transmit data in real-time across multiple applications and systems. It was initially developed at LinkedIn and later open-sourced as an Apache project.
- In simple terms, think of Kafka as a highly efficient and reliable "central nervous system" for your data. It serves as a middleman that facilitates communication between various parts of your software infrastructure, enabling seamless data flow and communication between different applications or microservices.

## Apache Kafka: Use Cases
- Messaging System
- Activity Tracking
- Applicatio Logs gathering
- Stream processing

## Examples of Kafka in use:
- Netflix uses kafka to real-time recommendations while watching TV shows.
- Uber use kafka to gather user, taxi and trip data in real time to compute demand and surge in pricing.
- LinkedIn uses kafka to prevent spam and collect user interation to make better connection recommendations in real time.

## Kafka Topics
- In simple terms, a Kafka topic is like a virtual channel or inbox where data is stored and organized within Apache Kafka. It's a named feed to which data can be sent and from which data can be read. You can have as many kafka topics created within you kafka cluster. A kafka topic is identified using its name given to it at its creation. Each topic can hold any kind of message format such as json, XML, CSV and many more. The sequence of messages within a topic is called a `data stream`. You are not able to query a topic like a database. Instead `Kafka Producers` is used to send data and `Kafka Consumers` is used to reaf data from these topics.

## Partitions and offsets
- `Partitions`: Each kakfa topic can be divided into 1 more partitions. Data related to a particular category or key is stored in a specific partition. Imagine a library with different shelves for different genres of books (e.g., Fiction, Non-fiction, Mystery). Each shelf is like a partition, and books related to a specific genre are placed in their respective shelves. This way, the library can handle a large collection of books more efficiently.
- `Offset`: An offset is like a bookmark that keeps track of where you left off reading in each partition. It's a unique number assigned to each message within a partition, like a page number in a book. When you read a message from a partition, you remember the offset (page number) of that message. The next time you come back to read, you start from the offset you remembered, so you don't miss any messages. It helps you keep your place and read messages in order. Each meesage within a partition gets an incremental ID which is referred to as an offset.
- IMPORTANT -> Kafka topics are `immutable`: once data is written to a partition, it cannot be changed.
![](./Images/truck_gps_kafka.png)
- Diagram above shows a new truck_gps topic recieving multiple truck gps locations every 20 seconds and storing them within the 10 partitions created. Then making this data available for different services such as location dashboard or notification service. 

## Important Notes
- Once data is written to a partition, it cannot be changed `(immutability)`.
- Data is kept only for a limited time (default is one week - configurable).
- Offset only have a meaning for a specfic partition:
    - E.g. offset 3 in partition 0 doesnt represent the same data as offset 3 in partition 1.
    - Offsets are not re-used even if previous messages have been delete, they just keep incrementing.
- Order is guranteed order within a partition (not across partitions).
- Data us assigned randomly to partitions unless a `key` is provided.
- You can have as many partitions per topic as you want.