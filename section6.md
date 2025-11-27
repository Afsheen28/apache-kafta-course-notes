## Section 6: Kafka Producer - Spring Boot Microservice

### Introduction to Kafka Producer
* The producer acts as a Spring Boot microservice that publishes events to a Kafka Broker.
* The main duties of a Kafka producer are to publish messages to Kafka topics and serialize these messages into byte arrays for network transmission.
* When sending messages, the producer must specify the Kafka topic name, with an example being a 'product created event topic' for product events.
* The lecture discusses the option to specify a partition for message storage. If not specified, messages are distributed automatically in a round-robin manner.
* Additionally, if a key is provided, Kafka hashes it to direct messages with the same key to the same partition.
* Another responsibilty of Kafka producer is to choose which partition to write event data to.

### Introduction to synchronous communication style
![IMG_0009](https://github.com/user-attachments/assets/c6dd8782-f0c3-45cb-abe7-771e6e3eb691)
* Synchronous communication: a method where the sender waits for a response after sending a message.
* An example is provided involving a mobile application that communicates with a Spring Boot microservice called the orders microservice.
* This microservice supports actions like creating, viewing, canceling, or tracking orders through API endpoints.
* In synchronous communication, when the mobile app wants to create an order, it sends an HTTP request and waits for confirmation from the orders microservice.
* The mobile app remains on hold until it receives a response indicating whether the order was successfully created.
* The implementation involves the Kafka Template class, which simplifies communication with Kafka within the Spring Boot microservice.
* The orders microservice generates an "Order Created Event" and uses the Kafka Producer to send this message to the Kafka Broker.
* The orders microservice waits for an acknowledgment from the Kafka Broker to confirm that the message has been successfully stored before moving on to further operations.
* This synchronous approach is considered reliable because it ensures that messages are stored successfully before proceeding.
* The waiting period for the acknowledgment may slow down the overall system performance.
* Asynchronous communication allows the Kafka Producer to send a message without waiting for a response, leading to higher throughput and lower latency.

### Kakfa Producer - A use case for asynchronous communication style
![image0 (5)](https://github.com/user-attachments/assets/9f90a510-766f-4307-84ff-a8f6ff0293c3)
* Asynchronous communication: where the producer sends a message and continues processing without waiting for an acknowledgment from the Kafka Broker.
* In asynchronous communication, when a message is sent, the producer does not block the execution. It can perform other operations while the Kafka Broker processes the message.
* The Kafka Producer’s main tasks include serializing messages and sending them to specified Kafka topics. The topic can be a dedicated event topic, like a "product created event" topic.
* When sending a message, the producer may specify a partition or send it without specifying one. If neither is specified, messages are distributed among available partitions in a round-robin fashion for a balanced load.
* Asynchronous communication improves the performance of the application, allowing for higher throughput since the producer does not wait for a response before proceeding.
* A callback can be implemented to handle the result of message sending. This allows the application to be notified about the success or failure of the message delivery once the acknowledgment from the broker is received.
* Asynchronous communication is ideal for scenarios where immediate acknowledgment is not critical, enabling the application to remain responsive and handle more operations concurrently.

### Creating a new Spring Boot App
* Following Tutorial

### Kafka Producer configuration properties
* Following Tutorial

### Creating Kafka Topic
* Following Tutorial


