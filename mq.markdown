

CONTENT

1. What are Message Queues?
2. What is asynchronous communications protocol?
3. The need for Message Queues
4. Use cases of Message queues
5. Types of Message Queues
6. RabbitMQ 
7. CloudMQ
8. Resources 
9. Works cited 

-------------------------------------------------------------

1. What are Message Queues? 

Message queues are software-engineering components. In a system utilizing message queues, messages are queued up in sequential order. These messages are communicated between applications. An example of a message could relate to triggering a process to start by a receiving application. The content of the message could be a plain message, information about a task that has completed or one that is about to start. This is represented in a byte array with some headers included.
One of the most important aspects of message queues is their asynchronous nature. This means that message queues are able to have a process running without blocking for responses or results. 


2. What is asynchronous communications protocol?

Asynchronous operation is non-blocking and only initiates an operation instead of waiting for a  process to complete. One of the needs for asynchronous message passing is to achieve parallelism. While the message sent is still in transit, the computation can be done on the message. If a process is receiving a message, it can signal its interest in receiving messages on multiple ports simultaneously. In synchronous communication, such parallelism can be achieved by forking a separate process for each concurrent operation, but this requires more process management. Its main concept is to offload the processing to another thread or process to release the calling thread from waiting and it has become a standard model since the rise of GUIs.(1)

3. The need for Message Queues/ What problem do they solve?

Message queues are an answer to the limitations of synchronous communication. Asynchronous communication such as email separates the operation of sending from receiving. This can be described as decoupling. The official term for the sender and receiver is producer and consumer. It is important to note that these two programs might be on completly different servers. The producer produces a message and sends it. Initially, the message goes to the message queue. Once the message is added to the queue, the consumer is able to access the message according to the queue order. This way, the producer and consumer do not have to access the message queue at the same time. 
![Message Queues](img/message-queue-example.png)
The separation of the requesting and receiving program also makes for self-contained small programs which are relatively easier to maintain and modify.  
Other benefits of message queues include: 
Message-driven processing:Triggering can be used to start an application upon message arrivals. 

Event-driven processing: Programs can be controlled according to the state of queues. An example would be specifying the number of messages that should be on the queue before a certain program starts. 

Message priority: Messages can be assigned priority which determines their position on the queue. This can be done by the program adding messages. Other programs will be able to access a message on a queue based on its priority.(2)

4.  Use cases of Message Queues 

Web services 
One example of a use case is message queues in a web service. A web service that is able to request and return data might use a message queue. This makes the web service highly available to accepting requests regardless of the number of the request being sent. Furthermore, another message can access the message queue and handle the messages in order. This is all without having to wait for any process to get a response. This web service would be easy to scale up with growing workload because all that is needed to be done is adding more workers, receivers, to work off the queues faster (1). 

5. Types of Message Queues
There are various implementations of message queues. Each has its own benefits and shortcomings. Below are a few of them. 
RabbitMQ
Kafka
RocketMQ
Qpid
Artemis
NSQ
ZeroMQ
CloudMQ 
When trying to choose one, developers usually consider the maturity of the project, community, and performance. In this chapter, we will expound more on RabbitMQ and CloudMQ. 

6. RabbitMQ 
The official website for RabbitMQ defines it as a message broker. In other words, it is a message oriented middleware. 

“Message Oriented Middleware is a concept that involves the passing of data between applications using a communication channel that carries self-contained units of information (messages). In a MOM-based communication environment, messages are usually sent and received asynchronously. Using message-based communications, applications are abstractly decoupled; senders and receivers are never aware of each other. Instead, they send and receive messages to and from the messaging system. It is the responsibility of the messaging system (MOM) to get the messages to their intended destinations.” (Chappell)

When to use RabbitMQ

Message queueing is especially useful to increase the performance of web services instead of having to wait on resource-heavy procedures to finish before accepting and processing other requests. “ Message queueing is also good when you want to distribute a message to multiple recipients for consumption or for balancing loads between workers.”(6)

RabbitMQ also complies with an open standard called messaging middleware called Advanced Message Queuing Protocol (AMQP). Middleware products written for different platforms and in different languages can send messages to one another. “AMQP enables complete interoperability for messaging middleware; both the networking protocol and the semantics of broker services are defined in AMQP”.(8)

There are usually two parts that make a RabbitMQ application. The producer program and the consumer program. The producer is the program that sends the messages and the consumer is the program that waits to receive a message. Sometimes an application can be both producer and consumer. (8)

![Producer & Consumer](img/producerConsumer.png)

RabbitMQ - A simple example in Java
1. Make sure you have installed RabbitMQ on your machine 
Instructions here: https://www.rabbitmq.com/download.html
2. Download the client library and its dependencies for Java (SLF4J API and SLF4J Simple). Copy those files in your working directory or a full-blown logging library like Logback in production for a more complete project. 
3. Send.java

import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;

public class Send {

    private final static String QUEUE_NAME = "hello";

    public static void main(String[] argv) throws Exception {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        try (Connection connection = factory.newConnection();
             Channel channel = connection.createChannel()) {
            channel.queueDeclare(QUEUE_NAME, false, false, false, null);
            String message = "Hello World!";
            channel.basicPublish("", QUEUE_NAME, null, message.getBytes("UTF-8"));
            System.out.println(" [x] Sent '" + message + "'");
        }
    }
}

4. Recv.java
	import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import com.rabbitmq.client.DeliverCallback;

public class Recv {

    private final static String QUEUE_NAME = "hello";

    public static void main(String[] argv) throws Exception {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setHost("localhost");
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();

        channel.queueDeclare(QUEUE_NAME, false, false, false, null);
        System.out.println(" [*] Waiting for messages. To exit press CTRL+C");

        DeliverCallback deliverCallback = (consumerTag, delivery) -> {
            String message = new String(delivery.getBody(), "UTF-8");
            System.out.println(" [x] Received '" + message + "'");
        };
        channel.basicConsume(QUEUE_NAME, true, deliverCallback, consumerTag -> { });
    }
}

5. To compile both sender.java and reciever.java , we use the RabbitMQ java client on the classpath:


javac -cp amqp-client-5.5.1.jar Send.java Recv.java

6. To run them, we’ll need rabbitmq-client.jar and its dependencies on the classpath. In a terminal, run the consumer (receiver):
java -cp .:amqp-client-5.5.1.jar:slf4j-api-1.7.25.jar:slf4j-simple-1.7.25.jar Recv

then, run the publisher (sender):
java -cp .:amqp-client-5.5.1.jar:slf4j-api-1.7.25.jar:slf4j-simple-1.7.25.jar Send

7. To list the queues 
 	On Windows : rabbitmqctl list_queues
	On Mac : sudo rabbitmqctl list_queues

Source: https://www.rabbitmq.com/tutorials/tutorial-one-java.html
-----
7. CloudMQ
CloudAMQP are managed RabbitMQ servers in the cloud-hosted message queues that let you pass messages between processes and other systems. Messages are published to a queue by a producer, the consumers can then get the messages from the queue when the consumer wants to handle the messages. In-between, it can route, bucffer, and persist the messages according to rules you give it.
![CloudMQ](img/camqp.png)
Creating a CloudAMQP instance
In order to create an instance, we need to sign up for a plan that fits our needs. As of 04/2019, one can try CloudAMQP for free with the plan Lemur.
CloudAMQP monitoring tools include diagrams for CPU and Memory usage. It is possible to activate alarms to be triggered when a part of the system is heavily used and it is easy to view the RabbitMQ log stream directly in CloudAMQP.














