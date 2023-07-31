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

## Kafka Producers 
- Producers are what write data to topics (which are made of partitions).
- Producers know which partition to write to (and which kafka broker has it).
- In case of kafka broker failures, producers will automatically recover.
![](./Images/kafka_producers.png)

## Producers: Message keys
- Producers can choose to send a key with the message in the format of strings, numbers, binary, etc....
- If `key = null` then data is sent round robin (partition 0, then 1, then 2...).
- If `key != null` then all messages with that key will always go to the same partition (hashing).
- A key are typically sent if you need message ordering for a specific field (ex: truck_id).
![](./Images/producers_keys.png)

## Kafka message anatomy
![](./Images/Kafka_message.png)
- A kafka message created by a producer comes with many parts. At the top are key and value which can both be null or store data. Both of these are in binary format when being sent to apache kafka. Underneath that we have the compression type which may be used if data needs to be made smaller. Below that we have the Headers which contains key value pairs about the data within the message. For example a header can contain information of the origin of the message such as the application or system. We then have which partition the data will be held at and a new unique offset is allocated to the data. The last thing is the timestamp of when the message was created this can be given by the system or set by a user. Once all of these are made and put together it is ready to be sent to apache kafka.