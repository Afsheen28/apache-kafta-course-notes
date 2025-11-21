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

### Appendix A: Run Apache Kafka in a Docker Container (check VS Code in Users/affsi/kafka/docker-compose/docker-compose.yml)
* Apache Kafka KRaft Configuration:
 * Node ID: Each broker in the Kafka cluster requires a unique 'node ID.' The instructor demonstrates assigning sequential node IDs (1, 2, 3) to different Kafka server nodes.
 * Cluster ID: This is a unique identifier for the entire Kafka cluster. The instructor shows how to generate this alphanumeric string using the Apache Kafka command line interface (CLI) and stresses that all Kafka servers in the Docker compose file must share this same cluster ID.
 * Process Roles: The configuration specifies that the Kafka server will act as both a controller and a broker, handling cluster coordination and metadata, as well as message storage and client communication.
 * Controller Quorum Voters: The role of controller quorum voters is discussed, which involves maintaining the cluster's state and enabling failover mechanisms. The quorum voters serve as the decision-making body for the Kafka cluster, ensuring its smooth operation.
 * Quorum Voter Definition: The format for defining each quorum voter is explained, including details such as node ID, hostname, and port number. While suggested port numbers are provided, the instructor mentions they can be adjusted as necessary.
* Apache Kafka Listeners Configuration:
 * Listeners Property: This property defines the network interfaces, port numbers, and protocols that producers and consumers use to connect to Kafka brokers.
 * Types of Listeners:
  * Plaintext: For unencrypted communication within the Kafka cluster.
  * Controller: For internal coordination among brokers.
  * External: Allows external clients to connect to the Kafka broker, typically set on port 9092.
 * Advertised Listeners: These are the addresses that clients use to connect to Kafka. The plaintext listener is configured for internal communication, while the external listener uses 'localhost' for local connections and can be further customized with environment variables.
 * Security Protocols: The lecture discusses the 'listener security protocol map' property, which ties listener names to their security protocols. Although all listeners start with 'plaintext' for development, it emphasizes that SSL should be used for production environments to enhance security.
 * Custom Naming and Configuration: The lecture concludes by covering how to configure custom names for the controller listener and define listeners for inter-broker communication, reinforcing the use of 'plaintext' for internal communications.
* Kafka Docker Constrainer: Volumes
 * Volume Definition: It discusses how to define volumes by specifying the source path on the host machine and the target path in the Docker container, allowing Kafka to read and write data directly to the specified directory.
 * Data Consistency: Emphasizing data consistency and reliability, the instructor stresses the importance of correctly configuring volumes, particularly in a production environment where data integrity is crucial.
* Stopping Apache Kafka brokers
 * Avoid losing messages
 * Avoid errors
 * Better to stop producers and consumers before shutting down Kafka servers.
* Stopping Kafka Servers
 * CTRL + C (Windows) -> will shut down but not gracefully and could lead to problems.
 * CLI Script -> kafka-server-stop.bat -> graceful shutdown
 * `contolled.shutdown.enable=true` -> controlled shutdown of Kafka servers enabled in Kafka by default.
 * If you want to manually configure or control the value of this property, then you can add it to Kafka's `server.properties` file. 
