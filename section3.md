## Section 3: Kafka CLI: Topics

### Introduction to Kafka Topics CLI
* Kafka Topics CLI is used to manage and interact with topics within Kafka cluster using command line interface.
* Using windows so file name will be `kafka-topics.bat`.
* Can use this file to create a new topic, list, describe a topic or give you information about a topic.
* Can be used to delete topic and also modify an existing topic.

### Creating a new Kafka topic
* To create a new topic, go to the kafka -> bin -> windows file directory.
* Once you start all the three docker servers by running `docker compose up -d`, create a topic by running `./kafka-topics.bat --create --topic topic1 --partitions --replication-factor 3 --bootstrap-server localhost:9092,localhost:9094`.
* When creating topics, remember to:
  * Set a unique name
  * Name should be meaningful
  * Can have letters, digits, but they cannot have special characters.
  * Number of partitions depend on business needs. However, they must be equal or greater than the number of consumers.
* For example, if you create a topic with three partitions, then you can have 1,2 or 3 consumers running and consuming data from this topic. But if you only have 1, then when the consumer dies, you won't have another consumer to read data from the topic.
* Starting more consumers will mean you have more partitions in a topic, then extra consumers will sit idle.
* The replication factor parameter specifies how many copies of each partition are stored across different brokers or different servers in the Kafka cluster.
* This is the way to achieve reliability or fault tolerance in Kafka Cluster because it allows data to be available even if some brokers fail or become unreachable.
* For example, when setting the replication factor to three, each partition will have three replicas. One of which will be the leader and the other two will be followers.
* But the number specified here cannot be greater than the number of brokers that you have in your cluster.
* If you start three servers in a cluster, then you can specify replication factor three, and each broker will have a copy of a partition.
* With the bootstrap parameter, we can specify a list of addresses of Kafka brokers in our cluster. So here we can provide multiple servers separated with comma, or even though I have multiple servers running in a cluster, I can provide the addresses of one initial server only. For example, localhost.
* For windows, localhost might not work as expected. May need to try `--bootstrap-server [::1]:9092` instead.
* If initial server is healthy, this CLI script or Kafka client will still be able to discover other servers and access topics and partitions.
* If initial server is not healthy; if it goes down or if it becomes unreachable, then I will not be able to create a topic until this broker is back online.
* When using CLI scripts or when using Java code to create topics, it is recommended to provide addresses of at least two brokers so that you can have a backup broker in case one broker is not available.

### List and describe Kafka topics
* To list topics, run `./kafka-topics.bat --list --bootstrap-server localhost:9092`.
* To describe the topics, run `./kafka-topics.bat --describe --bootstrap-server localhost:9092`.
* The --list parameter displays only the names of the topics, without any additional information about them.
* To get more detailed information about each topic, the --describe parameter is introduced. This command reveals crucial details such as:
  * Topic name
  * Topic ID
  * Number of partitions
  * Replication factor
  * Maximum size of topic segments
* Kafka stores messages in logs that are divided into segments. Each segment has a configurable maximum size, and new segments are created once a segment reaches its maximum size.
* It touches on the structure of topics, which consist of multiple partitions. Each partition is managed by a leader broker responsible for processing read and write requests.
* The concept of replicas is discussed, referring to copies of partition data stored on other brokers to ensure fault tolerance. The term 'ISR' (In-Sync Replicas) is explained, highlighting those replicas that are current with the leader and can take over in case the leader fails.

### Deleting Kafka Topic
* Deleting a Kafka topic is a permanent action. Once a topic is deleted, it cannot be recovered unless a backup exists.
* The command to delete the topic is `./kafka-topics.bat --delete --topic topic1 --bootstrap-server localhost:9092`.
* After executing the command, users can confirm successful deletion by checking the list of existing topics, where the deleted topic should no longer appear.
* The lecture highlights a potential issue that may arise if a topic does not delete. Users should check the 'delete.topic.enable' configuration property in the server configuration file. If this property is set to false, it will prevent any topic deletion.
