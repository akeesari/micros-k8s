# Azure Event Hubs for Apache Kafka Introduction Part-1

## What is Azure Event Hubs?

Azure Event Hubs is a cloud-based, scalable data streaming platform provided by Microsoft Azure. It is designed to ingest and process large volumes of streaming data from various sources in real-time. Event Hubs is part of the Azure messaging services and is particularly well-suited for scenarios involving event-driven architectures and big data analytics.

Key features of Azure Event Hubs include:

1. **Event Ingestion:** Event Hubs can handle millions of events per second, making it suitable for high-throughput scenarios. It allows you to ingest data from various sources.

2. **Scalability:** Azure Event Hubs is designed to scale horizontally, allowing you to increase throughput by adding more partitions. This makes it suitable for applications with varying workloads.

3. **Retention:** Event Hubs can retain data for a specified duration, allowing consumers to catch up on missed events. This is useful for scenarios where data needs to be analyzed retroactively.

4. **Publish-Subscribe Model:** Event Hubs supports a publish-subscribe model, where multiple consumers (subscribers) can independently read from the same event stream. Each consumer can choose its position in the event stream.

5. **Integration with Azure Services:** Event Hubs integrates seamlessly with other Azure services like Azure Functions, Azure Stream Analytics, Azure Databricks, and Azure Logic Apps, Azure Kubernetes services, enabling you to build comprehensive solutions.

6. **Security and Compliance:** Event Hubs provides security features such as Azure AD-based authentication and authorization, along with support for Virtual Network Service Endpoints. It also complies with various industry standards and regulations.

7. **Capture:** Event Hubs Capture allows you to automatically capture streaming data into Azure Blob Storage or Azure Data Lake Storage for long-term storage and batch processing.

8. **Availability and Reliability:** Event Hubs is designed for high availability and fault tolerance. It automatically replicates data across multiple availability zones within a region.

[![Alt text](images/event-hubs/image-1.png)](images/event-hubs/image-1.png){:target="_blank"}

## Can you explain Apache Kafka on Azure Event Hubs?

Apache Kafka on Azure Event Hubs is a managed service that combines the capabilities of Apache Kafka, a popular open-source distributed streaming platform, with the scalability and ease of use of Azure Event Hubs. This service is designed to provide a Kafka-compatible interface for developers who are familiar with Kafka, while also taking advantage of the benefits of Azure's fully managed cloud platform.



Key capabilities

1. **Kafka Protocol Compatibility:** This service supports the Kafka protocol, allowing you to use existing Kafka applications, tools, and libraries with minimal modifications. It provides a familiar Kafka experience for developers who are already familiar working with Kafka clusters.

2. **Managed Service:** Unlike traditional self-hosted Kafka clusters, Apache Kafka on Azure Event Hubs is a fully managed service. This means that Microsoft Azure takes care of the infrastructure, maintenance, and operational tasks, allowing you to focus more on your application development and less on managing the underlying Kafka infrastructure.

3. **Scalability:** Leveraging the underlying scalability of Azure Event Hubs, this service can handle high-throughput, real-time event streaming. It supports automatic scaling and can handle large volumes of data, making it suitable for scenarios with varying workloads.

4. **Integration with Azure Ecosystem:** Apache Kafka on Azure Event Hubs seamlessly integrates with other Azure services. This includes services like Azure Functions, Azure Stream Analytics, Azure Logic Apps, and Azure Databricks, allowing you to build comprehensive and scalable event-driven applications.

5. **Azure Security and Compliance:** The service benefits from Azure's security features, including Azure AD-based authentication, encryption in transit and at rest, and compliance with various industry standards and regulations.

6. **Multi-Protocol Support:** While it is Kafka-compatible, Azure Event Hubs also supports other protocols, enabling interoperability with various Azure services and simplifying integration into diverse application architectures.

7. **Geo-replication:** Azure Event Hubs provides geo-replication capabilities, allowing you to replicate your Kafka-enabled event streams across Azure regions for improved disaster recovery and high availability.

By offering Apache Kafka on Azure Event Hubs, Microsoft aims to provide organizations with the flexibility to use Kafka-based architectures in a managed and scalable environment, taking advantage of the benefits of Azure's cloud platform. This service is particularly useful for scenarios involving real-time data streaming, event-driven architectures, and applications that rely on the Kafka ecosystem.



## Apache Kafka and Azure Event Hubs conceptual mapping

The following table maps concepts between Kafka and Event Hubs.

**Kafka Concept** | **Event Hubs Concept**
--- | ---
Cluster | Namespace
Topic | An Event Hub
Partition | Partition
Consumer Group | Consumer Group
Offset | Offset

[![Alt text](images/event-hubs/image-2.png)](images/event-hubs/image-2.png){:target="_blank"}

## What is Azure Schema Registry?

Azure Schema Registry is a feature of Azure Event Hubs that offers a centralized repository for managing schemas in event-driven and messaging-centric applications. Here are key points to understand about Azure Schema Registry:

1. **Centralized Schema Repository:** Azure Schema Registry serves as a central location to store and manage schemas for the data exchanged between producer and consumer applications within the Azure Event Hubs ecosystem.

2. **Flexibility in Data Exchange:** The registry provides flexibility for producer and consumer applications, allowing them to exchange data without the need to manage and share the schema explicitly. This simplifies the communication process between different components of an application.

3. **Governance Framework:** Azure Schema Registry includes a governance framework that facilitates the management of reusable schemas. This framework defines relationships between schemas through a grouping construct known as schema groups.

4. **Schema-Driven Serialization Frameworks:** The registry supports schema-driven serialization frameworks, such as Apache Avro. This approach involves moving serialization metadata into shared schemas, which can help reduce the per-message overhead. Unlike tagged formats such as JSON, each message doesn't need to carry metadata, as the schema provides the necessary type information and field names.

<!-- [![Alt text](images/event-hubs/image-4.png)](images/event-hubs/image-4.png){:target="_blank"} -->

## What is Azure Event Hubs namespace?

An Azure Event Hubs namespace is a comprehensive management container that not only organizes event hubs but also provides essential features for access control, network integration, and security, making it a crucial administrative and operational construct for working with Azure Event Hubs.

1. **Management Container:** An Event Hubs namespace is described as a management container specifically designed for event hubs (or topics, in the context of Kafka). This reinforces the idea that the namespace serves as an organizational unit for grouping and managing related event hubs.

2. **DNS-Integrated Network Endpoints:** The namespace provides DNS-integrated network endpoints. This means that each namespace is associated with a unique DNS name, making it accessible via a unique domain name. This DNS name is used for addressing and interacting with the resources within the namespace.

3. **Access Control and Network Integration Features:**
   - **IP Filtering:** The namespace supports IP filtering, allowing you to control and restrict access based on IP addresses.
   - **Virtual Network Service Endpoint:** This feature enables integration with Azure Virtual Networks, providing a more secure and private communication channel between resources within the virtual network and the Event Hubs namespace.
   - **Private Link:** Private Link allows you to access the Event Hubs namespace over a private, dedicated connection instead of over the public internet. This enhances the security of data transfer.


## What is Event publishers?

"Event publishers" refer to entities that send data to an Azure Event Hub. The term "event publishers" is used synonymously with "event producers." These entities are responsible for generating and transmitting events or messages to an Event Hub, which is a central component of Azure Event Hubs.

Here are key points about event publishers:

1. **Definition:** An event publisher is any entity that produces and sends data to an Event Hub.

2. **Protocols:** Event publishers can use different protocols to publish events to Azure Event Hubs. The supported protocols include HTTPS, AMQP 1.0 (Advanced Message Queuing Protocol), and the Kafka protocol.

3. **Authentication and Authorization:** Event publishers need to authenticate and obtain authorization to publish events to an Event Hub. They can use Microsoft Entra ID based authorization with OAuth2-issued JWT tokens or an Event Hub-specific Shared Access Signature (SAS) token to gain publishing access. These mechanisms ensure secure and controlled access to the Event Hub.

4. **Token-based Authentication:** The use of OAuth2-issued JWT tokens or Event Hub-specific Shared Access Signature (SAS) tokens implies a token-based authentication approach. This means that event publishers must present a valid token as part of the authentication process to establish their identity and permissions.

## What is Event retention?

Event retention refers to the duration for which events or messages are stored within the system. It represents the period during which the system retains the event data, making it available for consumers to retrieve. Event retention is a crucial aspect of event-driven architectures, allowing organizations to balance the need for historical data analysis with storage considerations.

Here are key points about event retention:

1. **Configurability:** Event retention is typically a configurable setting that allows the user to specify how long events should be retained in the event streaming system. 

2. **Retention Period:** The retention period is the amount of time that events are kept in the system. 

3. **Maximum Retention Period:** Event streaming platforms often have a maximum retention period beyond which events are automatically purged. This maximum retention period is determined by the platform's design and storage capabilities.

4. **Automatic Removal:** Events are automatically removed from the system when the retention period for each event expires. Once an event is older than the specified retention period, it is no longer available for retrieval by consumers.

5. **Impact on Storage:** The configured event retention period has an impact on the storage requirements for the event streaming system. Longer retention periods require more storage space to retain historical events.

6. **Archival Strategies:** In scenarios where organizations need to retain events beyond the configured retention period, they may employ archival strategies. This could involve storing events in long-term storage solutions, such as Azure Storage or Azure Data Lake.


## What are Publisher policies?

Publisher policies refer to a feature that enables granular control over event publishers. 

Here are the key points:

1. **Granular Control:** Publisher policies in Azure Event Hubs provide a way to have fine-grained control over event publishers. This is particularly useful when dealing with a large number of independent event publishers.

2. **Unique Identifier for Publishers:** Each publisher using publisher policies is associated with its own unique identifier. This identifier is used when publishing events to an Event Hub. The format of the identifier follows a specific mechanism, as illustrated in the provided HTTP example:

    ```
    //<my namespace>.servicebus.windows.net/<event hub name>/publishers/<my publisher name>
    ```

3. **Dynamic Publisher Names:** Publisher names don't need to be created ahead of time. However, they must match the Shared Access Signature (SAS) token used when publishing an event. This matching ensures that each publisher has its independent identity.

4. **PartitionKey Matching:** When using publisher policies, the PartitionKey value needs to be set to the publisher name. This value ensures proper functioning, and both the publisher name and PartitionKey must match for the system to work as intended.

## Explain Capture events:

Event Hubs Capture enables you to automatically capture the streaming data in Event Hubs and save it to your choice of either a Blob storage account, or an Azure Data Lake Storage account. You can enable capture from the Azure portal, and specify a minimum size and time window to perform the capture. Using Event Hubs Capture, you specify your own Azure Blob Storage account and container, or Azure Data Lake Storage account, one of which is used to store the captured data. Captured data is written in the Apache Avro format.

<!-- [![Alt text](images/event-hubs/image-3.png)](images/event-hubs/image-3.png){:target="_blank"} -->

## What are Partitions:


Event Hubs organizes sequences of events into one or more partitions.
A partition can be thought of as a commit log that holds event data, including the body of the event, user-defined properties, metadata (such as offset and timestamp), and its position in the stream sequence.


partitions in Azure Event Hubs play a crucial role in enabling scalable and parallel processing of events. The choice of the number of partitions and the effective use of partition keys impact the system's throughput, scalability, and the ability to process events in an organized manner.

Advantages of Using Partitions:

- **Parallel Processing**: Partitions help with processing large volumes of events by allowing multiple parallel logs to be used for the same event hub. This multiplies the available raw I/O throughput capacity.
- **Scalability**: Partitions enable the scaling out of processing capacity. Your applications can handle the volume of events by having multiple processes, and partitions help feed those processes while ensuring each event has a clear processing owner.

## SAS tokens

Shared Access Signature (SAS) tokens in Azure Event Hubs provide a secure way to grant limited access rights to clients, allowing them to interact with Event Hubs while maintaining control over the permissions and duration of access.

## Event consumers

Event consumers in Azure Event Hubs are applications that connect to an Event Hub using the AMQP 1.0 protocol to receive and process event data. The use of AMQP facilitates efficient and reliable communication, and the delivery of events is handled asynchronously without the need for consumers to actively poll for data availability.

## Consumer groups

Consumer groups provide a logical grouping of consumers that read data from an event hub or Kafka topic.

Consumer groups enable multiple consuming applications to read the same streaming data independently at their own pace with their offsets.

This parallelization allows distributing the workload among multiple consumers while maintaining the order of messages within each partition.

It is recommended to have only one active receiver on a partition within a consumer group to avoid processing duplicate events.

consumer groups in Azure Event Hubs provide a flexible and scalable mechanism for parallelizing the consumption of streaming data, accommodating various downstream applications, and managing the details of event processing. The recommendation for having one active receiver per partition in a consumer group helps maintain order and avoid duplicate event processing.

## Application groups

An application group is a collection of one or more client applications that interact with the Event Hubs data plane. Each application group can be scoped to a single Event Hubs namespace or event hubs (entity) within a namespace.


Application groups are logical entities that are created at the namespace level. Therefore, client applications interacting with event hubs don't need to be aware of the existence of an application group. Event Hubs can associate any client application to an application group by using the identifying condition.


## Why Azure Event Hubs:

1. **Real-time Data Streaming:**
   Azure Event Hubs serves as the  real-time data streaming from the legacy system to our new microservices. This  ensuring that our organization stays responsive and adaptive to dynamic data changes.

2. **Event-driven Architecture:**
   Leveraging Azure Event Hubs enables us to embrace an event-driven architecture. This approach allows us to decouple components, ensuring that each microservice can independently consume data events relevant to its specific business domain.

3. **Scalability and Elasticity:**
   Azure Event Hubs provides the scalability and elasticity needed to handle large volumes of data events efficiently. This is crucial as our organization evolves, ensuring that our architecture can seamlessly grow to accommodate increased workloads.

4. **Publisher-Subscriber Model:**
   The publisher applications in our legacy system publish data events to dedicated event hubs within Azure Event Hubs. This follows a publisher-subscriber model, ensuring a structured and organized migration of data from the legacy system.

5. **Schema Consistency:**
   The utilization of serialization frameworks like Avro, coupled with Azure Schema Registry, guarantees consistency in data representation. This ensures that data maintains a uniform schema throughout its journey from the legacy system to the new microservices environment.

6. **Business Domain Relevance:**
   Consumer applications on the new microservices platform subscribe to specific event hubs, allowing them to receive real-time updates relevant to their designated business domains. This targeted approach enhances the efficiency and relevance of data consumption.

