# Section 2: Apache Kafka Broker(s)

### What is Apache Kafka Broker?
* A Kafka broker is a software application that runs on one or more machines (servers) and is responsible for accepting, storing, and managing messages in Kafka.
* Each Kafka topic consists of partitions where events are stored. The broker handles all read and write requests for these partitions, managing the flow of data effectively.
* Within a Kafka cluster, each partition has a leader broker that manages all operations, such as read and write requests. Other brokers act as followers, passively replicating the leader's data to ensure high availability and fault tolerance.
* If a leader fails, one of the follower brokers can take over as the new leader, allowing continuous operations without data loss.
* Kafka brokers are responsible for accepting messages from producers, reliably storing them in the corresponding topic partitions, and replicating these messages across the cluster as needed.
* They also enable consumer microservices to read events from these topic partitions.
* Kafka brokers are customizable, and their configurations can be adjusted according to specific requirements using configuration files.

### Apache Kafka broker: Leader and Follower roles/ Leadership balance
* Leader Broker:
  *  In a Kafka cluster, each topic is divided into partitions, and each partition has one broker acting as its leader. The leader is responsible for handling all read and write requests for that partition.
  * It can be compared to a store manager, where all customer requests must go through this manager. When a producer wants to send a message, it must reach the leader of the corresponding partition first.
* Follower Brokers:
  *  Follower brokers passively replicate the data from the leader broker. This replication occurs in the exact order that messages were written, ensuring consistency across the cluster.
  *  The role of followers is crucial as they maintain copies of the data to ensure data redundancy and high availability.
* Dynamic Leadership:
  * The leadership is not fixed; it can change based on the availability of brokers. If a leader broker goes down, one of the follower brokers will automatically take over as the new leader.
  * This dynamic reassignment helps maintain the availability of the system, preventing bottlenecks and ensuring that operations continue without interruption.
* Partition Management:
  * Each partition can have different brokers assigned as leaders across topics. This setup prevents a single broker from becoming a bottleneck and distributes the load effectively.
