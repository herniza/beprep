# Backend Developer Interview

## Table of Contents

* [General](#general)
* [Message Broker](#messagebroker)
* [Design Pattern](#designpattern)
* [RESTful API](#api)
* [Security](#security)
* [Caching](#caching)
* [Database](#database)
* [Performance](#performance)
* [Logging](#logging)
* [Microservices](#microservices)
* [Hashing](#hashing)

================================================================================
## General

**Synchronous:** executes tasks sequentially, where each task must be completed before the next one begins. It blocks the execution until the current task finishes
**Asynchronous:** allows tasks to run independently without waiting for each other to complete. It utilises callbacks, promises, or async/await to handle task completion and continue with other tasks.
**Scalability:** Scalability refers to a system’s ability to handle increasing workloads by adding resources. A scalable backend architecture involves designing components that can be distributed, replicated, or scaled horizontally to accommodate growing user demands.

## Design Pattern

**1. DRY - Don't Repeat Yourself**
A software development principle that states that developers should not duplicate code. Duplicated code can lead to maintenance issues because changes need to be made in multiple places.

**2. DIE - Duplication Is Evil**
Similar to the DRY principle. Still, it takes it one step further by stating that even slight amounts of duplication are inadequate and should be avoided.

**3. ACID - Atomicity, Consistency, Isolation, and Durability**
- Atomicity: ensures that a transaction is treated as a single, indivisible unit of work. Either all the changes made by the transaction are applied, or none of them are. If any part of the transaction fails, the entire transaction is rolled back to its original state, and no partial changes are persisted to the database. Atomicity is crucial for maintaining the consistency of the database.
- Consistency: ensures that a database remains in a consistent state before and after the execution of a transaction. A transaction should bring the database from one valid state to another, preserving the integrity of the data and adhering to predefined rules or constraints. If a transaction violates consistency constraints, it is rolled back, and the database returns to a consistent state.
- Isolation: ensures that the concurrent execution of multiple transactions does not result in interference or data corruption. Each transaction is executed in isolation from other transactions, as if it were the only transaction in the system. Isolation is achieved by various mechanisms such as locks and transaction isolation levels, which control the visibility of uncommitted changes to other transactions.
- Durability: guarantees that once a transaction is committed, its changes are permanent and will survive any subsequent system failures, such as power outages or crashes. Committed transactions are stored in a durable storage medium (typically disk), ensuring that they can be recovered and reapplied in the event of a system failure. Durability is a key aspect of ensuring the long-term reliability of a database.

**4. SOLID principles**
- Single Responsibility Principle (SRP):
The Single Responsibility Principle states that a class should have only one reason to change, meaning that a class should have only one responsibility or job. This principle encourages the division of functionality into small, focused, and independent classes. Each class should encapsulate a single responsibility, making the code more maintainable and reducing the impact of changes.
- Open/Closed Principle (OCP):
The Open/Closed Principle suggests that a class should be open for extension but closed for modification. This means that you should be able to add new functionality to a class without altering its existing code. This is typically achieved through the use of interfaces, abstract classes, and polymorphism. By adhering to the OCP, the code becomes more modular and easier to extend without affecting existing code.
- Liskov Substitution Principle (LSP):
The Liskov Substitution Principle states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, if a class is a subclass of another class, it should be able to be used interchangeably with its superclass without introducing errors. Adhering to the LSP ensures that the behavior of derived classes is consistent with that of their base classes.
- Interface Segregation Principle (ISP):
The Interface Segregation Principle emphasizes that a class should not be forced to implement interfaces it does not use. In other words, a class should not be burdened with methods it does not need. Instead of having large, monolithic interfaces, it's better to create smaller, more specific interfaces that clients can implement selectively. This principle helps in avoiding unnecessary dependencies and keeps the codebase more modular.
- Dependency Inversion Principle (DIP):
The Dependency Inversion Principle suggests that high-level modules (e.g., business logic) should not depend on low-level modules (e.g., data access) but rather both should depend on abstractions (e.g., interfaces or abstract classes). Additionally, abstractions should not depend on details; details should depend on abstractions. This principle promotes the use of dependency injection and inversion of control to achieve more flexible and loosely coupled systems.

## Message Broker

A message broker is a piece of software that helps to facilitate communication between different applications by acting as a middleman. Message brokers can provide a number of different functions, such as helping to ensure that messages are delivered in a timely manner, providing support for different message formats, and providing a way to track messages as they are being processed.

**Queue**: a message broker service that stores messages until they are ready to be processed

**Topic**: a message broker service that allows for messages to be published and subscribed to by interested parties.

**Open source message broker**: Apache Kafka (It is very fast and can handle a large number of messages. It is also very easy to set up and use. However, it is still a relatively new platform, so there are some stability issues that need to be ironed out.), RabbitMQ, and ActiveMQ

**Publish/Subscribe Messaging Systems**: a messages are sent out to subscribers who have registered an interest in receiving them

**Request/Reply Messaging Systems**: a message is sent to a specific recipient who then responds to the message.

**Have you worked with any message broker systems or queues?**
In my previous project, we used Apache Kafka as our message broker system. We leveraged Kafka’s publish-subscribe model and topic-based messaging to build a real-time data processing pipeline. By using Kafka, we were able to decouple data producers and consumers, handle high volumes of data, and achieve fault tolerance and scalability.

**What are some ways to make sure messages aren’t lost during transmission through a message broker?**
There are a few different ways to make sure that messages are not lost during transmission. One way is to use a message queue, which will store messages until they can be delivered. Another way is to use a message logging system, which will keep track of all messages that are sent and received.

**Why would I want to use a message queue instead of a database for data storage?**
Message queues and databases serve different purposes, databases are essential for persistent storage and retrieval of data. Here are some reasons why you might choose to use a message queue instead of a database for certain aspects of your system:

1. Asynchronous Communication:
Message queues are designed for asynchronous communication between different components or services. If you need to decouple components in your system, allowing them to communicate without being directly dependent on each other's availability, a message queue is a good choice.

2. Scalability:
Message queues can help with horizontal scalability. By using a message queue to handle communication between components, you can scale those components independently. This can be especially useful in microservices architectures where different services need to scale independently based on their load.

3. Event-Driven Architecture:
Message queues are well-suited for event-driven architectures. If your application relies on events or messages to trigger certain actions or processes, a message queue can efficiently handle the distribution of these events to interested parties.

4. Fault Tolerance:
Message queues can provide a level of fault tolerance by storing messages until they are successfully processed. This helps prevent data loss in case a component goes down temporarily or experiences issues. In contrast, databases may not be optimized for handling transient failures in the same way.

5. Loose Coupling:
Message queues promote loose coupling between components. A sender and receiver in a message queue system don't need to know the details of each other; they just need to understand the format of the messages being exchanged. This flexibility allows for easier changes and updates to individual components without affecting the entire system.

6. Handling Peak Loads:
Message queues can act as a buffer during peak loads. If the rate at which data is produced exceeds the rate at which it can be consumed, a message queue can temporarily store the excess data until it can be processed by the consuming components.

7. Priority Handling:
Message queues often support priorities, allowing you to handle critical tasks or messages with higher priority over others. This is useful in scenarios where certain messages need to be processed more urgently.

8. Distributed Systems:
Message queues are well-suited for distributed systems where components may be running on different nodes or even in different geographical locations. They facilitate communication in a way that accommodates the challenges of distributed architectures.

**Apache Kafka**
Apache Kafka is a distributed streaming platform designed for building real-time data pipelines and streaming applications. It is open-source and horizontally scalable, allowing it to handle large volumes of data and provide fault tolerance. Kafka is often used for building event-driven architectures and ingesting, storing, and processing high-throughput, fault-tolerant data streams.

Here's an overview of Kafka, including its key components (producer and consumer), and its pros and cons:

**Key Components:**
1. Producer:
The producer is responsible for publishing data to Kafka topics. A topic is a category or feed name to which records are published. Producers push records (messages) to topics, and Kafka ensures that these records are stored in a fault-tolerant manner.
2. Consumer:
Consumers subscribe to topics and process the records produced by the producers. They can read records in real-time or process historical data. Kafka allows multiple consumers to subscribe to the same topic, enabling parallel processing and scalability.
3. Topic:
A topic is a logical channel or category to which records are published. Topics allow for the organization and categorization of records. Producers publish records to topics, and consumers subscribe to topics to process the records.
4. Broker:
Kafka is designed as a distributed system, and a Kafka cluster consists of one or more brokers. Each broker is a server that stores data and serves client requests. Brokers work together to ensure fault tolerance and data replication.
5. Zookeeper:
Zookeeper is used by Kafka for distributed coordination and to manage the configuration of the Kafka brokers. It helps maintain the overall health and state of the Kafka cluster.

**Producer:**
1. Functionality:
Producers are responsible for sending messages to Kafka topics. They create records, attach them to a topic, and Kafka takes care of distributing and storing these records across the cluster.
2. Configuration:
Producers can configure properties such as the Kafka broker to connect to, the topic to which they are producing messages, and various other settings related to reliability and performance.

**Consumer:**
1. Functionality:
Consumers subscribe to Kafka topics and process the records (messages) published to those topics. Multiple consumers can form consumer groups to parallelize the processing of messages.
2. Offset Management:
Kafka maintains an offset for each consumer, representing the position in the log where the consumer has consumed up to. This allows consumers to keep track of their progress and resume from the last offset in case of failures.

**Pros of Kafka:**
1. Scalability:
Kafka is designed to scale horizontally, making it suitable for handling large amounts of data and high-throughput scenarios.
2. Durability:
Kafka ensures fault tolerance and data durability by replicating data across multiple brokers. Even if some brokers fail, the data remains available.
3. Real-time Processing:
Kafka supports real-time data streaming, making it suitable for building applications that require low-latency processing.
Decoupling of Producers and Consumers:
Producers and consumers in Kafka are decoupled, allowing for flexibility in the design of data pipelines.
4. Extensibility:
Kafka's architecture is extensible, and it integrates well with various data processing frameworks and analytics tools.

**Cons of Kafka:**
1. Complexity:
Setting up and managing a Kafka cluster can be complex, especially for small-scale deployments.
2. Learning Curve:
Due to its distributed nature and various configuration options, there is a learning curve associated with effectively using Kafka.
3. Resource Requirements:
Kafka may require significant hardware resources, especially for large-scale deployments, which can impact costs.
4. Overhead of Zookeeper:
Kafka relies on Zookeeper for cluster coordination, and managing Zookeeper can introduce additional operational overhead.
5. Not Suitable for All Use Cases:
While Kafka is powerful, it may be overkill for simple use cases where a lighter-weight messaging system could suffice.

# RESTful API
REST stands for Representational State Transfer, and it is an architectural style for designing networked applications REST is a set of principles and constraints that guide the design of web services and APIs (Application Programming Interfaces).

**Key Principles and Characteristics of REST**
1. Statelessness:
Each request from a client to a server must contain all the information needed to understand and process the request. The server should not store any information about the client's state between requests. This makes systems more scalable and easier to maintain.
2. Resource-Based:
Resources are the key abstractions in a RESTful architecture. A resource can be any entity that has a unique identifier (URI) and is manipulated through representations. Resources can represent data objects, services, or concepts.
3. Uniform Interface:
RESTful systems have a uniform and consistent interface, which simplifies the architecture and makes it easier for clients to interact with the server. The uniform interface is defined by a set of constraints, including:
**Identification of Resources:** Resources are identified by URIs.
**Manipulation of Resources through Representations:** Clients interact with resources through representations (such as JSON or XML).
4. Self-Descriptive Messages
Each message from the server to the client contains all the information needed to understand and process the message.
5. Client-Server Architecture:
The architecture is divided into a client and a server, each with distinct responsibilities. The client is responsible for the user interface and user experience, while the server is responsible for processing requests, managing resources, and handling business logic. This separation improves scalability and simplifies the overall architecture.
6. Cacheability:
Responses from the server can be explicitly marked as cacheable or non-cacheable. This helps improve performance by allowing clients to cache responses and reduce the need for repeated requests to the server.
7. Layered System:
A RESTful system can be composed of multiple layers, where each layer has a specific role and does not know the details of the layers above or below it. This allows for flexibility and easy maintenance.

**HTTP methods**
1. GET
The GET method is used to retrieve a representation of a resource. It should not have any side effects on the server, meaning it should only retrieve data and not modify it. GET requests are idempotent, meaning making the same request multiple times produces the same result.
2. POST
The POST method is used to submit data to be processed to a specified resource. It can be used to create a new resource or perform other actions with side effects on the server. POST requests are not idempotent, meaning making the same request multiple times can produce different results.
3. PUT
The PUT method is used to update a resource or create a new resource if it does not exist. It replaces the entire resource with the new representation provided in the request. PUT requests are idempotent.
4. PATCH
The PATCH method is used to apply partial modifications to a resource. It is often used when you want to update only specific fields of a resource rather than replacing the entire resource. PATCH requests are not guaranteed to be idempotent.
5. DELETE
The DELETE method is used to request the removal of a resource. After a successful DELETE request, the resource should no longer exist, and subsequent requests to that resource should result in a 404 Not Found. DELETE requests are idempotent.

**HTTP Request**
1. Headers: Metadata about the request or response. Used for conveying information such as content type, authentication, and user agent.
Example:
    ```
    GET /example/path HTTP/1.1
    Host: www.example.com
    Accept: application/json
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
    ```
2. Path Parameters: Part of the URL path. Used to identify a specific resource.
Example:
    ```
    GET /users/{userId}
    ```
3. Query Parameters: Appended to the URL after the "?". Used to provide additional information or filter requests.
Example:
    ```
    GET /users?role=admin&status=active
    ```
4. Body: The request body carries data that is sent to the server, typically for operations like creating or updating a resource. It is common in POST, PUT, and PATCH requests.
Example:
    ```
    POST /users
    Content-Type: application/json

    {
        "username": "john_doe",
        "email": "john@example.com"
    }
    ```

## Security

#### Authentication
The user submits their credentials (e.g., username/password) to the server. The server validates the credentials against stored user information.

#### Authorization
Authorization is the process of granting or denying access rights and permissions to users, systems, or entities based on their authenticated identity. While authentication verifies the identity of a user, authorization determines what actions or resources that authenticated user is allowed to access.

**Authorization Request Headers**
1. Basic Auth
It is a simple authentication scheme built into the HTTP protocol. The client sends HTTP requests with the Authorization header that contains the word Basic, followed by a space and a base64-encoded(non-encrypted) string username: password. Choose Basic Authentication when you need a simple, HTTP-based authentication method and are operating in a controlled environment with secure transmission (HTTPS). For example, to authorize as username / Pa$$w0rd the client would send.
    >Authorization: Basic AXVubzpwQDU1dzByYM==
2. Bearer Token
Bearer tokens are typically used in OAuth 2.0 authentication. After obtaining an access token through the OAuth 2.0 authorization process, the client includes the token in the "Authorization" header of API requests. Bearer tokens are susceptible to interception, and if leaked, an attacker can use them until they expire. Therefore, it is crucial to transmit bearer tokens over secure connections (HTTPS). Bearer tokens are commonly used for securing APIs that require access delegation and authorization, especially in scenarios where third-party applications need to access a user's resources. The client must send this token in the Authorization header while requesting to protected resources:
    > Authorization: Bearer *token*

    Similarly to Basic authentication, Bearer authentication should only be used over HTTPS (SSL). 
3. API Key:
An API key is a token that a client provides when making API calls. With API key auth, you send a key-value pair to the API either in the request headers or query parameters. Some APIs use API keys for authorization. Choose API keys when you need a lightweight and straightforward method for authenticating clients, especially for scenarios with lower security requirements.

    >X-API-Key: abcdefgh123456789

#### Hashing
Hashing is a one-way process where data is transformed into a fixed length alphanumeric string. This string is known as a hash or message digest. A hash cannot be reversed back to the original data because it is a one-way operation. Hashing is commonly used to verify the integrity of data, commonly referred to as a checksum. If two pieces of identical data are hashed using the same hash function, the resulting hash will be identical. If the two pieces of data are different, the resulting hashes will be different and unique. 

In addition to verifying the integrity of data, hashing is the recommended data transformation technique in authentication processes for computer systems and applications. It is recommended to never store passwords and instead store only the hash of the “salted password”. A salt is a random string appended to a password that only the authentication process system knows; this guarantees that if two users have the same password the stored hashes are different.

**Common Hash Functions:**
1. MD5 (Message Digest Algorithm 5):
MD5 Produces a fixed-size output of 128 bits (16 bytes). MD5 was once widely used, but vulnerabilities have been discovered, and it is now considered insecure for cryptographic purposes.
2. SHA-1 (Secure Hash Algorithm 1):
Similar to MD5, SHA-1 has vulnerabilities and is no longer considered secure for cryptographic use.
3. SHA-256, SHA-384, SHA-512:
SHA-256 Produces a fixed-size output of 256 bits (32 bytes). The numbers denote the bit size of the hash output. SHA is designed to be collision-resistant. As of now, it is considered secure against collision attacks, meaning it is unlikely for two different inputs to produce the same hash value. The larger the bit length, the more resistant the hash function is to brute-force attacks and other cryptographic attacks.
4. bcrypt, scrypt, Argon2:
These are cryptographic hash functions designed specifically for password hashing, incorporating features to slow down brute-force attacks.

#### Encode
Encoding data is a process involving changing data into a new format using a scheme. Encoding is a reversible process and data can be encoded to a new format and decoded to its original format. Encoding typically involves a publicly available scheme that is easily reversed. Encoding data is typically used to ensure the integrity and usability of data and is commonly used when data cannot be transferred in its current format between systems or applications. Encoding is not used to protect or secure data because it is easy to reverse.

**Example:** Base64 Encoding, URL Encoding

#### Encrypt
Encryption is the process of securely encoding data in such a way that only authorized users with a key or password can decrypt the data to reveal the original. Encryption is used when data needs to be protected so those without the decryption keys cannot access the original data. When data is sent to a website over HTTPS it is encrypted using the public key type. While encryption does involve encoding data, the two are not interchangeable terms, encryption is always used when referring to data that has been securely encoded. Encoding data is used only when talking about data that is not securely encoded. **There are two basic types of encryption: symmetric key and public key.** In a symmetric key, the same key is used to encrypt and decrypt data, like a password. In public key encryption, one key is used to encrypt data and a different key is used to decrypt the data.

**Types of Encryption:**
1. Symmetric Encryption
In symmetric encryption, the same key is used for both encryption and decryption. The challenge in symmetric encryption is securely sharing the key between communicating parties. Examples of symmetric algorithms include AES and DES.
2. Asymmetric Encryption (Public-Key Encryption)
Asymmetric encryption uses a pair of keys: **a public key for encryption and a private key for decryption**. Information encrypted with the public key can only be decrypted with the corresponding private key, and vice versa. Common asymmetric algorithms include RSA and Elliptic Curve Cryptography (ECC).
3. Hybrid Encryption
Hybrid encryption combines elements of both symmetric and asymmetric encryption. Typically, a symmetric key is used to encrypt the actual data, and the symmetric key itself is encrypted using asymmetric encryption. This approach leverages the efficiency of symmetric encryption and the key distribution advantages of asymmetric encryption.

#### Hashing vs Encode vs Encrypt
1. Security Requirements:
If the primary concern is data integrity, use hashing. For confidentiality, use encryption.
2. Representation vs. Protection:
If you need to represent data in a different format, use encoding. If you need to protect data, use encryption.
3. Reversibility:
Hashing is a one-way process, while both encoding and encryption are reversible (given the right keys for encryption).
4.  Performance:
Hashing is typically faster than encryption, as it is a simpler operation. Encoding and encryption can introduce additional computational overhead.

#### JWT (JSON Web Token)
It is a compact, URL-safe means of representing claims between two parties. JWTs are often used for authentication and authorization purposes in web development and APIs. They are easy to transmit and can be verified by the recipient to ensure the integrity and authenticity of the information they contain. 

**Structure of a JWT:**
A JWT consists of three parts separated by dots (.), and each part is base64-encoded JSON:
1. Header:
Contains information about how the JWT is encoded, such as the type of token (JWT) and the signing algorithm being used.
2. Payload (Claims):
Contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.
3. Signature:
The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way. It is created by combining the encoded header, the encoded payload, a secret key, and the specified algorithm.

**How JWT Works:**
1. Authentication:
When a user logs in, a server creates a JWT containing information about the user (claims) and sends it back to the client.
2. Authorization:
The client includes the JWT in the header of subsequent requests to access protected resources on the server.
3. Verification:
The server, upon receiving a request with a JWT, verifies the signature using a shared secret key. If the signature is valid, the server trusts the information in the JWT.

## Caching
Caching in a backend system involves storing and retrieving frequently accessed data or computed results in a temporary storage location known as a cache. The primary purpose of caching is to improve the performance and responsiveness of a backend system by reducing the need to repeatedly perform expensive or time-consuming operations.

**Key Purposes of Caching**
1. Faster Response Times:
Caching allows a backend system to serve requests more quickly by providing immediate access to previously generated or retrieved data. Instead of recalculating or fetching the same information repeatedly, the system can return the cached result, leading to faster response times.
2. Reduced Load on Resources:
Caching helps in offloading the workload from backend resources, such as databases, external APIs, or computation-intensive processes. By serving cached data, the backend system can minimize the number of redundant and resource-intensive operations, which is particularly beneficial during periods of high traffic.
3. Improved Scalability:
As the load on a backend system increases, caching can contribute to improved scalability. By reducing the demand on critical resources, the system can handle more requests concurrently without experiencing a proportional increase in response times or resource utilization.
4. Bandwidth Savings:
Caching can also save bandwidth by storing static or semi-static content close to the end-user. This is often the case with content delivery networks (CDNs) that cache static assets like images, stylesheets, and scripts at edge locations, reducing the need for repeated downloads across the entire network.
5. Enhanced User Experience:
Faster response times and reduced latency contribute to an improved user experience. Caching is especially valuable for applications that require real-time or near-real-time interactions, such as web applications, where users expect quick responses to their actions.
6. Availability and Reliability:
Caching can enhance the availability and reliability of a backend system by providing a fallback mechanism during temporary failures or downtimes of dependent services. If a particular service or resource is temporarily unavailable, the system can still serve cached content, ensuring a more robust user experience.
7. Customized Content Delivery:
Caching can be used to store personalized or frequently accessed content for specific users or user groups. This allows the system to deliver tailored content quickly, contributing to a more personalized and responsive user interface.
8. Cost Savings:
By minimizing the need for repeated, resource-intensive operations, caching can lead to cost savings in terms of reduced infrastructure requirements. This is particularly relevant in cloud environments where resource usage is often tied to cost.

**When to use Caching?**
1. Frequent Read Operations:
Caching is particularly useful when there are frequently performed read operations on data that doesn't change often. By caching the results of these reads, subsequent requests can be served much faster.
2. Database Query Results:
When dealing with databases, caching can be employed to store the results of frequently executed database queries. This reduces the load on the database server and speeds up response times for users.
3. Expensive Computations:
If certain computations or operations are resource-intensive and don't change frequently, caching the results can save significant processing time.
4. Static Content:
Caching is commonly used for static content like images, stylesheets, and JavaScript files in web applications. This reduces the need to repeatedly download the same content, improving page load times.
5. API Responses:
Caching can be applied to store the responses of API calls. This is beneficial when the data returned by the API doesn't change frequently and can be reused for subsequent requests.
6. Session Data:
Storing session data in a cache can improve the performance of web applications. Rather than fetching session information from a database or another storage medium, it can be quickly retrieved from the cache.
7. Frequent Queries in Distributed Systems:
In a distributed system, caching can be employed to reduce the need for inter-service communication by storing the results of queries or computations that are shared among services.
8. Dynamic Content with TTL:
Even for dynamic content, caching can be effective if the content doesn't change rapidly. Time-to-Live (TTL) can be set to refresh the cache after a certain period, ensuring that the data is periodically updated.
9. User Authentication and Authorization:
Caching user authentication and authorization tokens can help reduce the load on identity providers and speed up the authentication process.
10. Frequently Accessed Code or Configurations:
Caching can be applied to store the results of function calls, code snippets, or configuration settings that are frequently accessed.

## Database

**Relational Databases (RDBMS):** 
1. Data Structure:
RDBMS stores data in tables with rows and columns. Each table represents an entity, and relationships between entities are established through foreign keys.
2. Schema:
RDBMS follows a predefined schema where the structure of the database, including tables, columns, data types, and relationships, is defined in advance. Changes to the schema may require altering the database, which can be a complex operation.
3. Data Integrity:
RDBMS enforces ACID (Atomicity, Consistency, Isolation, Durability) properties, ensuring data integrity and reliability. Transactions are used to guarantee that database operations are executed accurately.
4. Scalability:
Traditional RDBMS systems can be scaled vertically by adding more resources (CPU, RAM) to a single server. However, scaling horizontally across multiple servers can be challenging.
5. Query Language:
Structured Query Language (SQL) is used to interact with relational databases. SQL provides a powerful and standardized way to perform queries, updates, and data manipulations.
6. Use Cases:
RDBMS is well-suited for applications where data relationships are clearly defined, and the schema is stable. Common use cases include transactional systems, financial applications, and systems with complex queries and reporting needs.

**Non-Relational Databases (NoSQL):**
1. Data Structure:
NoSQL databases can store data in various formats, such as key-value pairs, documents, wide-column stores, or graphs. The structure is often more flexible, allowing for dynamic and evolving schemas.
2. Schema:
NoSQL databases often do not enforce a fixed schema. This flexibility allows developers to add fields to documents or change the structure without requiring a predefined schema alteration.
3. Data Integrity:
NoSQL databases may relax some of the ACID properties to achieve greater performance and scalability. Some NoSQL databases prioritize eventual consistency over immediate consistency.
4. Scalability:
NoSQL databases are designed for horizontal scalability, making them well-suited for distributed and large-scale applications. Many NoSQL databases can scale out across multiple nodes easily.
5. Query Language:
NoSQL databases use various query languages, APIs, or mechanisms to interact with the data. Unlike SQL in RDBMS, there is no standardized query language for NoSQL databases, and it varies based on the type of database.
6. Use Cases:
NoSQL databases are suitable for scenarios where data is unstructured or semi-structured, and the system needs to scale horizontally. Common use cases include content management systems, real-time big data applications, and scenarios where rapid development and flexibility are prioritized.

**SQL vs NoSQL**
1. Data Relationships:
If data relationships are well-defined and the schema is stable, an RDBMS may be a better fit. If the application deals with unstructured or rapidly changing data, a NoSQL database might be more appropriate.
2. Scalability:
If the application requires easy horizontal scalability, NoSQL databases are often chosen. RDBMS systems may require more careful planning for scaling.
3. Flexibility and Development Speed:
NoSQL databases offer more flexibility in terms of schema and can be more suitable for agile development where requirements evolve rapidly.

#### Indexing in Database
In a database, an index is a data structure that improves the speed of data retrieval operations on a database table. It is created on one or more columns of a table to quickly locate and access rows that match a certain condition or criteria. Indexing is a critical aspect of database optimization, especially for tables with a large volume of data.

**Pros of Indexing:**
1. Faster Data Retrieval:
The primary advantage of indexing is that it significantly speeds up data retrieval operations. When a query filters or sorts data based on the indexed columns, the database engine can use the index to locate the relevant rows more quickly, resulting in faster query performance.
2. Improved Query Performance:
Queries that involve conditions in the WHERE clause, ORDER BY clause, or joins can benefit from indexing. The use of indexes can reduce the time it takes for the database engine to process these queries, leading to improved overall system performance.
3. Efficient Sorting and Grouping:
Indexes are particularly useful for sorting and grouping operations. If a query requires sorting or grouping by an indexed column, the presence of the index allows the database engine to perform these operations more efficiently.
4. Enhanced Concurrency:
In some cases, indexing can improve concurrency by reducing the time it takes to read or locate specific rows. This can lead to better performance in multi-user environments where multiple transactions are being executed simultaneously.
5. Covering Indexes:
A covering index is an index that includes all the columns needed to satisfy a query, eliminating the need to access the actual data rows. This can further enhance query performance by reducing the I/O operations required to retrieve the necessary information.

**Cons of Indexing:**
1. Increased Storage Overhead:
Indexes consume additional storage space. For large tables, the size of the indexes can be significant, and this may impact the overall storage requirements for the database.
2. Overhead During Write Operations:
While indexes speed up read operations, they can introduce overhead during write operations (e.g., INSERT, UPDATE, DELETE). When data in the indexed columns is modified, the corresponding indexes must be updated, which can affect the performance of write-intensive operations.
3. Maintenance Overhead:
Indexes require maintenance, and as data in the table changes, the indexes need to be updated to reflect those changes. This maintenance overhead can impact the performance of write operations.
4. Inappropriate Indexing Strategy:
Choosing the wrong columns for indexing or having too many indexes on a table can lead to suboptimal performance. It's important to carefully plan and design indexes based on the specific queries and workload patterns of the application.
5. Index Fragmentation:
Over time, indexes can become fragmented, especially in scenarios where data is frequently modified. Fragmentation can reduce the effectiveness of indexes, and periodic maintenance is required to address this issue.

## Performance
**How would you optimise the performance of a slow database query?**
In my experience as a backend developer, optimising the performance of a slow database query involves a systematic approach to identify and address bottlenecks. I usually follow these steps:

1. Using New Relic to see which query that performs slow from a process (GraphQL query, mutation etc)
2. Analyse the query execution plan to identify inefficiencies.
3. Evaluate and create indexes based on the query’s clauses.
4. Optimise the query itself by minimising functions, selecting only necessary columns, and simplifying joins.
5. Consider data normalisation or denormalisation to improve performance.
6. Tune database configuration parameters for optimal resource utilisation.
7. Implement caching to reduce repetitive queries.
8. Explore load balancing and scaling options for heavy workloads.
9. Continuously monitor and profile query performance for iterative improvements.

## Logging

**How would you handle handling and logging errors in a backend application?**
I prioritise both proactive and reactive strategies to ensure effective error management and logging.

Proactively, I believe in implementing thorough error-handling mechanisms throughout the application code. This involves using try-catch blocks or error middleware to catch and handle exceptions gracefully. By leveraging appropriate exception-handling techniques, I ensure that error messages are informative and provide enough context to aid in troubleshooting. I also strive to create custom error classes or enums that encapsulate specific error types, making it easier to categorise and manage different types of errors.

To maintain a robust logging system, I prefer using a centralised logging framework or library. This allows me to capture relevant error details, such as the timestamp, the specific module or component where the error occurred, and any relevant input or request data. Logging this information helps in debugging and root cause analysis, enabling faster resolution of issues.

When it comes to logging errors, I follow the practice of logging at different severity levels, such as INFO, WARN, and ERROR, depending on the impact and urgency of the error. I make sure that detailed error logs are recorded for critical errors that require immediate attention, while less severe errors are logged with sufficient information for monitoring and analysis.

Furthermore, I find it beneficial to integrate error monitoring and alerting systems. By using tools like exception trackers or logging platforms, I can proactively monitor the occurrence of errors in real time and receive notifications when critical errors are encountered. This allows for timely response and minimises potential downtimes.

Overall, my approach to handling and logging errors revolves around a proactive mindset, leveraging robust exception handling, comprehensive logging practices, and integrating error monitoring tools. By adopting these strategies, I’m looking to enhance application stability, expedite debugging, and continuously improve the user experience.

# Microservices
Microservices architecture is an approach to software development where a large application is broken down into smaller, independent services that can be developed, deployed, and scaled independently. Each microservice is designed to perform a specific business function and communicates with other services through well-defined APIs. This architectural style is contrasted with monolithic architecture, where an application is built as a single, tightly integrated unit.

**Key Characteristics:**
1. Modularity:
Microservices break down the application into small, self-contained services. Each microservice focuses on a specific business capability or function. This modularity makes it easier to understand, develop, deploy, and scale individual components.
2. Independence:
Microservices operate independently of each other. Each service can be developed, deployed, and scaled independently without affecting other services. This independence allows for more agility and flexibility in the development and release process.
3. Decentralized Data Management:
Each microservice can have its own database, and the services communicate through APIs. This decentralization of data management helps avoid tight coupling between services and allows for different services to use different types of databases based on their specific needs.
4. Resilience:
Microservices are designed to be resilient. If one service fails, it should not bring down the entire application. The failure of one microservice does not necessarily impact the operation of others. This is achieved through techniques such as redundancy, graceful degradation, and fallback mechanisms.
5. Scalability:
Microservices can be independently scaled based on demand. If a specific service experiences increased traffic, it can be scaled horizontally (by adding more instances) without affecting the entire application. This allows for better resource utilization and cost efficiency.
6. Technology Diversity:
Each microservice can be developed using different programming languages, frameworks, and technologies, as long as they communicate through well-defined APIs. This flexibility allows teams to choose the best tools for their specific service, fostering innovation and optimization.
7. Continuous Delivery and Deployment:
Microservices are well-suited for continuous integration, delivery, and deployment practices. Because services are independent, updates to one service can be released without requiring changes to the entire application. This supports a faster and more frequent release cycle.
8. Easy Maintenance:
Microservices are easier to maintain than monolithic applications because each service is a standalone unit. Updates and changes to one service do not impact others, making it easier to manage and evolve the application over time.

**Cons of Microservices:**
1. Increased Complexity:
Microservices introduce a higher level of complexity compared to monolithic architectures. The need for managing multiple services, each with its own database, dependencies, and APIs, can make the overall system more complex. This complexity may require additional effort in terms of design, development, and operations.
2. Inter-Service Communication:
Microservices need to communicate with each other, and managing this communication can be challenging. Issues such as network latency, service discovery, load balancing, and data consistency across services must be carefully addressed. Over-reliance on network communication can introduce potential points of failure.
3. Data Management:
Maintaining data consistency across microservices can be complex, especially when each service has its own database. Coordinating transactions and ensuring data integrity becomes a challenge. Solutions such as distributed transactions can be complex and may impact performance.
4. Operational Overhead:
The operational overhead of managing a distributed system is higher compared to a monolithic architecture. Tasks such as monitoring, logging, deployment, and orchestration become more complex. Tools like container orchestration systems (e.g., Kubernetes) are often used but come with their learning curve and operational challenges.
5. Initial Development Cost:
The initial development cost of a microservices architecture can be higher. Developing and deploying multiple services, setting up infrastructure, and implementing communication mechanisms require additional effort and resources. Smaller projects or teams may find this initial investment challenging.
6. Service Discovery and Orchestration:
Managing service discovery and orchestration becomes crucial as the number of microservices grows. Tools and mechanisms for dynamically discovering services, handling failures, and maintaining a coherent system state add complexity.
7. Consistency and Transactional Boundaries:
Ensuring consistency across microservices, especially in scenarios requiring distributed transactions, can be challenging. Deciding on transactional boundaries and handling eventual consistency may lead to complex design decisions.
8. Security Concerns:
Security in a microservices architecture can be more challenging due to the distributed nature of the system. Implementing and managing security measures consistently across all services requires careful consideration.
9. Debugging and Troubleshooting:
Debugging and troubleshooting distributed systems can be more complex than monolithic systems. Identifying the root cause of issues, tracking down failures, and ensuring proper logging across multiple services can be challenging.
10. Team Coordination:
Effective communication and coordination are crucial in a microservices environment. Teams working on different services need to coordinate changes, APIs, and releases to ensure compatibility and avoid breaking changes.

# Source to Lookout
1. https://landing.jobs/blog/backend-interview-questions/

MISS
- design pattern
- kenapa memilih tdd
- ORM
- sql query (perbedaan join)
- ci/cd
- https://github.com/arialdomartini/Back-End-Developer-Interview-Questions/blob/master/README.md

https://www.linkedin.com/feed/update/urn:li:activity:7095065160544833536?utm_source=share&utm_medium=member_desktop

https://blog.hubspot.com/website/backend-interview-questions

https://www.fullstack.cafe/blog/backend-developer-interview-questions

https://www.fullstack.cafe/blog/backend-developer-interview-questions

https://www.simplilearn.com/tutorials/nodejs-tutorial/nodejs-interview-questions




