

CONTENT

1. What are Message Queues?
2. What is asynchronous communications protocol?
3. The need for Message Queues
4. Use cases of Message queues
5. Types of Message Queues
6. RabbitMQ 
7. CloudMQ

-------------------------------------------------------------

1. What are Message Queues? 

Message queues are software-engineering components. In a system utilizing message queues, messages are queued up in sequential order. These messages are communicated between applications. An example of a message could relate to triggering a process to start by a receiving application. The content of the message could be a plain message, information about a task that has completed or one that is about to start. This is represented in a byte array with some headers included.
One of the most important aspects of message queues is their asynchronous nature. This means that message queues are able to have a process running without blocking for responses or results. 


2. What is asynchronous communications protocol?

Asynchronous operation is non-blocking and only initiates an operation instead of waiting for a  process to complete. One of the needs for asynchronous message passing is to achieve parallelism. While the message sent is still in transit, the computation can be done on the message. If a process is receiving a message, it can signal its interest in receiving messages on multiple ports simultaneously. In synchronous communication, such parallelism can be achieved by forking a separate process for each concurrent operation, but this requires more process management. Its main concept is to offload the processing to another thread or process to release the calling thread from waiting and it has become a standard model since the rise of GUIs.

3. The need for Message Queues/ What problem do they solve?

Message queues are an answer to the limitations of synchronous communication. Asynchronous communication such as email separates the operation of sending from receiving. This can be described as decoupling. The official term for the sender and receiver is producer and consumer. The producer produces a message and sends it. Initially, the message goes to the message queue. Once the message is added to the queue, the consumer is able to access the message according to the queue order. This way, the producer and consumer do not have to access the message queue at the same time. 

The separation of the requesting and receiving program also makes for self-contained small programs which are relatively easier to maintain and modify.  

Other benefits of message queues include: 
Message-driven processing:Triggering can be used to start an application upon message arrivals. 

Event-driven processing: Programs can be controlled according to the state of queues. An example would be specifying the number of messages that should be on the queue before a certain program starts. 

Message priority: Messages can be assigned priority which determines their position on the queue. This can be done by the program adding messages. Other programs will be able to access a message on a queue based on its priority. 

4.  Use cases of Message Queues 

Web services 

One example of a use case is message queues in a web service. A web service that is able to request and return data might use a message queue. This makes the web service highly available to accepting requests regardless of the number of the request being sent. Furthermore, another message can access the message queue and handle the messages in order. This is all without having to wait for any process to get a response. This web service would be easy to scale up with growing workload because all that is needed to be done is adding more workers, receivers, to work off the queues faster (1). 







