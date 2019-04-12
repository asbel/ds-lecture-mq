

CONTENT

1. What are Message Queues?
2. What is asynchronous communications protocol?
3. The need for Message Queues/ What problem do they solve?
4. Types of Message Queues
5. Use cases of Message queues / How Message Queues are used 
6. RabbitMQ 
7. CloudMQ

-------------------------------------------------------------

1. What are Message Queues? 

Message queues are software-engineering components. In a system utilizing message queues, messages are queued up in sequential order. These messages are communicated between applications. An example of a message could relate to triggering a process to start by a receiving application. The content of the message could be a plain message, information about a task that has completed or one that is about to start. This is represented in a byte array with some headers included.
One of the most important aspects of message queues is their asynchronous nature. This means that message queues are able to have a process running withouth blocking for responses or results. 


2. What is asynchronous communications protocol?

One of the needs for asynchronous message passing  is to achieve  parallelism. While the message sent is still in transit, computation can be done on the message. If a process is recieving a message, it can signal its interest in receving messages on multiple ports simultaneously. In synchronous communication such parallelism can be achieved by forking a separate process for each concurrent operation, but this requires more process management. 
