## Notes on Apache Kafta Course Section 1 - Introduction to Apache Kafka

### What is a Microservice? 
* Small autonomous Application
* Responsible for specific functionality (search, email notification, SMS notification). Can be a small standalone application or a very small building block of a larger application.
* Loosely coupled, designated to scale horizontally and work in the cloud. 

### Microservice VS Monolithic Application
* Monolithic Application: A single stream of application with multiple controls. Each controller is responsible for a certain domain.
* The controllers are part of one single application.
* Also works within one database
* If there are any changes made to a controller, you will need to re-build and re-deploy the whole application.
* Communication between the controllers is possible as they are all under one application.
  * Users Controller -> working with users
  * Products Controller -> working with products
  * Orders Controller -> working with orders

* Microservices
* Multiple smaller applications
* Each controller is responsible for its own functionality.
* Can work on different servers.
* Works within it's own database (database service pattern).
  * Users Microservice -> working with users resource/ user details (edit user data)
  * Products Microservice -> working with products 
  * Orders Microservice -> working with orders
*  If a microservice needs to get restarted, the products and orders microservice will not be affected and could still run.
*  In order to communicate between two microservices is by sending HTTP requests.

### Microservices Communication 
![Microservice-communications-diagram](https://github.com/user-attachments/assets/619e64b7-7861-4b25-8fe0-945c59abdab8)
* This diagram shows several microservice applications working together as a large system.
* Each of these microservice is small web service that runs on its own unique port number.
* There are two client applications. One of them could be a mobile device that uses REST API, or it can be a web application working in a web browser.
* Both of these client applications communicate with web service over HTTP by sending HTTP requests.
* The blue box below shows a Spring Cloud; which is an environment that supports microservices.
* Inside the Spring Cloud is a microservice named an API Gateway; which is also a standalone spring boot application.
* Then we have the Config Server and Service Discovery; which are also spring boot applications that are built to serve a certain purpose.
* Below there are several other microservices that we have also designated and built as small spring boot applications, each performing specific business logic.

* In Microservice architecture, if products microservice needs to read information about orders, it does not query orders database directly. This is not allowed. Instead, it xan send a HTTP request and then receive a HTTP response.
* If Orders Microservice needs to process payment, it does not work with payments database directly. Instead, it can sent a HTTP request and wait for HTTP response.
* One of the ways microservices can communicate with each other is by sending direct HTTP request and wait for HTTP response.

### Synchronous HTTP Request 
* Imagine we have Albums Microservice that needs to fetch a list of photos from Photos Microservice.
* To do that, Albums Microservice will send a HTTP GET request to API endpoint for the Photos Microservice (HTTP GET: /albums/4h8dheul/photos).
* The Photos Microservice with send a HTTP response of JSON array of photos to Albums Microservice (HTTP Response: JSON with a list of photos).
* This is an example of Synchronous HTTP request

### Asynchronous HTTP Request
* Image we have an Orders Microservice and it needs to notify Email Notfication Microservice that a new order is created.
* To do that, Orders Microservice will send HTTP POST request to Email Notification Microservice API endpoint (HTTP POST: /email).
* But in this case, Orders Microservice will not wait for immediate response from Email Notification Microservice. It can send the request and immediately continue executing next job without waiting for Email Notification Microservice to respond.
* In this case, there is a way for Orders Microservice to still receive HTTP reaquest from Email Notification Microservice. Even if the request was sent asynchronously, its response will come a bit later.
* Because the execution inside of Orders Microservice already moved on to the next job, it needs to be prepared to handle this job. 

### Example
* If Orders Microservice needs to notify multiple other microservices about the fact that a new order was created, it is very important to make sure that each of these Microservices does receive the request. We could directly send a HTTP request to all 4 microservices (SMS Notification Microservice, Push Notification Microservice, Email Notification Microservice, Call Center Notification Microservice).
* If Email Notification Microservice is temporarily not available or will become available at a later time, there could be a problem with the Email Notification Microservice that needs to be fixed, or needs fixing.
* Since Email Notification Microservice will be down for many hours, it can lose many HTTP requests that Order Microservice will be sending it. We do not want to be loosing any notifications from Orders Microservice.
* Another challenge with direct HTTP requests between Microservices is when we want other Microservices to join later and also start receiving notifications from other Microservices.
* If tomorrow we start shipping Microservices, then for Orders Microservice to send direct HTTP requests to this new Microservice, we will need to update Orders Microservice so that it knows where to send the HTTP request.
* And if we have another Microservice that joins later, and we also want this new Microservice to receive notifications from Orders Microservice, then for direct HTTP communication to work, we will need to update Orders Microservice again.

### Event-driven Microservices
* Micrservices that need to communicate a message to multiple other microservices will publish a message to Apache Kafta Topic.
* Microservices that are interested in receiving this message will receive it from Apache Kafta Topic as soon as the message becomes available.
* This model is often called Producer/Consumer or Publisher/Subscriber, where Product Microservice that is on the left side is a Producer and Microservices on the right side are Consumers.
* As soon as Publisher publishes the message, all subscribed Microservices will receive the message and be able to process it.
* This is a very scalable and very extensible architecture.
* Microservices are loosely coupled and they are completely location transparent to each other.
* For example, if the Products Microservice received a request to create a new product, the Products Microservice is done processing this request and a new product is created, it will publish an event (ProductCreatedEvent), and then all Microservices that are interested in receiving notification about this event will receive this event and will be able to act on it. 
* Since in this architecture, Microservices communicate with each other by means of publishing and consuming messages or events, this is called event-driven architecture.
* Most likely asychronous and loosely coupled.
* For example, when publishing, Microservice publishes event to a Topic, and it responds back to a calling application right away.
*  It does not wait until every single subscribing Microservice received and successfully processed this event.
*  The publishing Microservice is not aware how many subscribing Microservices there are, and it does not know how many of them have successfully handled the event.
*  Microservices that consume events do not send back a direct response to a publisher microservice.
*  If one of the subscriber Microservices is down, the publishing Microservice will not know about it. But when subscriber Microservice comes back and resumes working, it will still be able to consume published event. This is another advantage of this architecture over a direct request and response communication.
*  In the case of direct request and response communication, if destination microservice is down, then most likely it will miss the message and will no longer receive it.
*  In the event-driven architecture with Apache Kafta, if one of the subscribed microservices was down, it will still be able to consume events that it missed when it comes back online and resumes working.

### Apache Kafta for Event-Driven Spring Boot Microservices
* Apache Kafta is a distributed event streaming platform that is used to collect, process, store and integrate data at scale.
* The main components discussed are producers and consumers.
* Producers: Microservices that publish events (e.g., new product creation) to Kafka.
* Consumers: Microservices that subscribe to these events.
* Kafka Broker: Acts as the mediator that manages the communication between producers and consumers, ensuring events are stored durably in Kafka Topics.
* Topics: These serve as reliable storage for events, ensuring data replication across multiple servers, which enhances fault tolerance.
* Dynamic Scaling: Consumer microservices can dynamically scale based on workload, emphasizing the importance of parallel processing of events for improved performance.

### Messages and Events in Apache Kafka (DRAM DIAGRAM)
* In Apache Kafka, an event is an indication that something has happened. For example, a user logging in or a new product being created.
* When naming conventions, they need to be in a certain order:
 * In past tense
 * Start with a Noun, followed by the action performed and end with the word `Event`.
* For example, `ProductCreatedEvent` or `ProductDeletedEvent`.  
* The difference between Kafka messages and events are:
 * A Kafka message is described as an envelope that carries the event data, which can be in various formats like strings, JSON, Avro or even null if necessary. -> anything that can serialised into an array of bytes. -> can be a request for a future action.
 * A message is the unit of data Kafka stores and transports.
 * An event represents a completed action.
 * An event is an application-level concept that represents something that happened in your domain. It is carried inside a Kafka message.
* The default size for a Kafka message payload is one megabyte. The lecture advises optimising this size to enhance system performance, especially when managing large files (e.g., images, videos) by using links instead of including large files directly.
* A message consists of three main components:
 * Message Key: Used for identifying messages, can be of various data types.
 * Message Value: Contains the event data.
 * Timestamp: Records when the event occurred.
 * Headers: Optional key-value pairs that can include extra metadata, such as authorisation details.

### Kafka Topic and Partitions
* A Kafka topic is defined as a storage area for all published messages.
* DRAW DIAGRAM
* Instead of sending messages directly to these microservices, the products microservice publishes events to a specific Kafka topic, which serves as an intermediary.
* Topic Partitions
 * Each topic can be divided into multiple partitions.
 * These partitions are replicated across Kafka servers to ensure data durabililty and availability, even in the event of a server failure.
 * Partitioning enables consuming microservices to read data in parallel, which enhances throughput and scales application performance.
 * For example, when a topic has multiple partitions, different instances of a microservice can process events simultaneously, thus improving overall processing speed.
* Creating topics and managing partitions: 
 * The number of partitions can be increased after a topic is created, it cannot be decreased.
 * Each partition acts as a small storage unit, wherein events are stored sequentially in an append-only manner. 
* Event offsets:
 *  Each event within a partition is assigned an offset, akin to an index in an array, which uniquely identifies its position within that partition.
 *  It’s essential to note that once an event is stored in a partition, it is immutable: meaning it cannot be changed, deleted, or updated.
* Retention policy:
 * The retention policy of Kafka topics, which retains events for a default period of seven days.
 * This duration can be adjusted based on specific configuration needs, but the immutable nature of events combined with the retention policy is crucial for maintaining a reliable event log. 
