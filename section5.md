## Section 5: Kafka CLI: Consumers

### Introduction to Kafka Consumer CLI
* Working with `kafka-console-consumer.bat` script.
* The main use of `kafka-console-consumer.bat` is to fetch and display messages from a Kafka topic to your terminal.
* Can read new messages only or read all messages from the beginning.

### Consuming messages from Kafka topic from the beginning
* To see the messages under a topic, run `./kafka-console-consumer.bat --from-beginning --bootstrap-server localhost:9092 --topic my-topic`
* Once you run this, an output showing all messages sent to this topic will be shown. However, you will not be able to quit from the cluster and will be able to see messages being sent in real time through running the producer script. 
* This line of code is `./kafka-console-producer-bat --from-beginning --bootstrap-server localhost:9092 --topic my-topic`. 
* Once you run this code and enter the cluster, you will be able to send messages to the topic while the consumer receives it and shows it in real time.
* Multiple consumers can read the same messages from a topic simultaneously, highlighting Kafka's ability to retain messages even after they have been consumed. This allows for scenarios where multiple Spring Boot microservices can subscribe to the same topic.

### Consuming new Kafka messages only
* To see new messages only under a topic, you run the same command as before but without the `--from-beginning`. The line will be `./kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic my-topic`. This will make you enter the cluster and won't show you any new messages until you send one in real time. 
* To send one, run `./kafka-console-producer.bat --bootstrap-server localhost:9092 --topic my-topic`. Once in the cluster, run a message and see it appear in the consumer terminal simulataneously. 
  
### Consuming Key:Value pair messages from Kafka topic
* Kafka messages can be structured as key-value pairs, where both the key and value can be different data types, including null. This flexibility allows for diverse use cases in data representation.
* To allow key-value pairs to be represented within the messages of a topic, enable the producer command to something like this: 
`./kafka-console-producer.bat --bootstrap-server localhost:9092 --topic my-topic --property "parse.key=true" --property "key.separator`. This is to enable the key:value support. 
* The `key.separator` parameter is to configure the separator between the key and the value.
* Once you run the consumer command: `./kafka-console-consumer.bat --topic my-topic --bootstrap-server localhost:9092`, you will be able to see the messages in a value from the key:value pair by default. This is why you do not need to add any extra parameters.
* After going back to the terminal with the producer command, once you get into the cluster and send a message; for example `firstName:Afsheen`, you are able to see the consumer terminal recieve the message but only displays the value pair (Afsheen).
* To allow the consumer command to output both the key:value pair, you add an extra parameter: `./kafka-console-consumer.bat --topic my-topic --bootstrap-server localhost:9092 --property print.key=true`. 
* Then, once run, type a message in the producer terminal. For example, `lastName:Siddiqui`. Your output on the console terminal should be `lastName          Siddiqui`.
* To print both key and value from the message into the consumer terminal, enter the line of code `./kafka-console-consumer.bat --topic my-topic --bootstrap-server localhost:9092 --property print.key=true --property print.value=true --from-beginning`. 
* This allows the messages to appear right from the beginning in a key:value pair format. 
* The messages sent without a key have been provided with a null key.
* To only see the key from the messages, enter this line: ``./kafka-console-consumer.bat --topic my-topic --bootstrap-server localhost:9092 --property print.key=true --property print.value=false --from-beginning`.

### Consuming Kafka messages in order
* Kafka topics are divided into multiple partitions. The way messages are stored within these partitions plays a critical role in ensuring their order upon consumption.
* A crucial aspect discussed is that Kafka uses a hash of the message key to determine which partition a message will be stored in. If a message does not have a key, it is distributed among the partitions via a round robin algorithm, which can disrupt the order of messages.
* To ensure that messages are consumed in the order they were produced, they must share the same key. This is illustrated through the creation of a Kafka topic named 'Messages order' with three partitions and three replicas.
* The lecturer demonstrates how to produce messages using the same key (e.g., a user ID). By doing this, all messages with that key are stored in the same partition, thereby preserving the order.
* The lecture contrasts this with messages sent using different keys, which are distributed across various partitions, leading to the potential for altered order during consumption.
* Those with the same key maintain their original order, while those with different keys do not.
* Firstly, create a topic: `./kafka-topics.bat --create --topic messages-order --partitions 3 --replication-factor 3 --bootstrap-server localhost:9092`.
* Then send messages with the same User ID: `./kafka-console-producer.bat --topic messages-order --bootstrap-server localhost:9092 --property "parse.key=true" --property "key.separator=:"`.
* Then run the consumer command to see the order of the messages: `./kafka-console-consumer.bat --topic messages-order --bootstrap-server localhost:9092 --from-beginning --property print.key=true`.
* Those with the same ID are all in order compared to messages with different IDs. Those that have no keys are given a null value by default.
