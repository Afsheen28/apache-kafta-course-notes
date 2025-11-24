## Section 4: Kafka CLI: Producers

### Introduction to Kafka Producer CLI
* The main use of kafka-console-producer.bat script is to send a message to a particular Kafka topic.
* For example:
  * Send message with a Key.
  * Send message without a Key,
  * Send multiple messages from a file.
* The primary responsibility of Kafka producers is to serialize messages into a binary format that can be transmitted over the network to Kafka brokers.
* Producers must specify the name of the Kafka topic where the messages should be sent. For example, a topic might be designated for "product created events."
* When sending messages, users can specify a partition where the message should be stored. Although this is optional, if not specified, Kafka will handle it using a round-robin method or by hashing the message key, ensuring all messages with the same key go to the same partition.
* The producer automatically distributes messages across multiple partitions if neither a specific partition nor a key is provided. This helps in load balancing the messages.

### Producing Kafka Message without a Key
* Sending messages without a key allows the Kafka producer to automatically distribute messages across the available partitions of a topic. This is useful for balancing load and ensuring that messages are spread out evenly.
* The lecture covers the concept of asynchronous message sending, where the producer does not wait for a response from the broker after sending a message. This improves performance, enabling the application to continue processing without delay.
* When a message is successfully sent, the producer can retrieve metadata associated with the message. This metadata includes information about which topic the message was sent to, the partition used, and the offset of the message within that partition.
* To run the command, it is `./kafka-console-producer.bat --bootstrap-server localhost:9092,localhost:9094 --topic my-topic`.
* Since I had no topic named `my-topic`, I was first given an error but when I tried running the code again, it worked.
* This is because Kafka defaultly creates a new topic with the given name if it didn't exist before.
* Once the code ran, I typed in a message in the terminal `Hello World`. This had gone through.
* To stop sending messages, do `ctrl+c`.

### Sending Kafka Message as a Key:Value pair
* Messages in Kafka can be sent as key-value pairs, where the key is used to categorize or group related messages. This is important for ordering purposes, as messages with the same key are stored in the same partition.
* Sending messages with a key ensures that all messages belonging to the same group (identified by the key) are stored in the same partition. This guarantees that they will be read in the order they were produced, as Kafka maintains the order of messages within a partition.
* Utilizing keys when sending messages is emphasized not only for message ordering but also for ensuring that related messages are processed together, which can be beneficial for various application use cases.
* The line of code is `./kafka-console-producer.bat --bootstrap-server localhost:9092,localhost:9094 --topic my-topic --property "parse.key=true" --property "key.separator=:"`.
* For example, if i send a message like `firstName:Afsheen`, it will work due to the colon as a separator.
* If I try, `test message`, I'll get an error as there is no separator in the message.
