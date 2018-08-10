# java-cpp-activemq
Proof of Concept for a project including an application in C++ being called from a Java web application housed in Tomcat over Linux.

Environment:

Windows 7 <br/>
Java Runtime Enviroment 1.8.181 <br/>
Tomcat 9.0.10 + ActiveMQ 5 <br/>

Java web application deployed in Tomcat <br/>
C++ application linked with ActiveMQ-CPP (the Apache CMS API library implementation) running on Windows <br/>

Step-by-step

1. Download and install JRE
2. Download Tomcat and unzip it into a specific folder
3. Configure Tomcat to not pick up Jetty WebSocket ServerContainerInitializer class (See References section).
3. Download ActiveMQ and unzip it into 

References:

1. http://www.oracle.com/technetwork/pt/java/javase/downloads/jre8-downloads-2133155.html

Java Runtime Environment, or JRE™.

2. http://activemq.apache.org/download.html


2. http://activemq.apache.org/tomcat.html

Configuration issues for Tomcat 7 and later.
Tomcat needs to be configured to ignore Jetty SCI annotations so that the Jetty WebSocket ServerContainerInitializer class is not inadvertently picked up by Tomcat. For more information on this problem see AMQ-6154 and https://wiki.apache.org/tomcat/HowTo/FasterStartUp and consult the Tomcat documentation for the version you are using to properly exclude the Jetty jar files from being scanned by Tomcat.

2.a. https://issues.apache.org/jira/browse/AMQ-6154

BUG Resolved: ActiveMQ with websocket on Tomcat fails with NullPointerException at org.eclipse.jetty.websocket.jsr356.server.deploy.WebSocketServerContainerInitializer

2.b. https://wiki.apache.org/tomcat/HowTo/FasterStartUp

This section provides several recommendations on how to make your web application and Apache Tomcat as a whole to start up faster.


4. http://activemq.apache.org/cross-language-clients.html

Apache ActiveMQ is a message broker written in Java with JMS, REST and WebSocket interfaces, however it supports protocols like AMQP, MQTT, OpenWire and STOMP that can be used by applications in different languages.

5. http://activemq.apache.org/cms/

CMS (stands for C++ Messaging Service) is a JMS-like API for C++ for interfacing with Message Brokers such as Apache ActiveMQ. CMS helps to make your C++ client code much neater and easier to follow. To get a better feel for CMS try the API Reference. ActiveMQ-CPP is a client only library, a message broker such as Apache ActiveMQ is still needed for your clients to communicate.

Our implementation of CMS is called ActiveMQ-CPP, which has an architecture that allows for pluggable transports and wire formats. Currently we support the OpenWire and Stomp protocols, both over TCP and SSL, we also now support a Failover Transport for more reliable client operation. In addition to CMS, ActiveMQ-CPP also provides a robust set of classes that support platform independent constructs such as threading, I/O, sockets, etc. You may find many of these utilities very useful, such as a Java like Thread class or the "synchronized" macro that let's you use a Java-like synchronization on any object that implements the activemq::concurrent::Synchronizable interface. ActiveMQ-CPP is released under the Apache 2.0 License.

6. http://activemq.apache.org/version-5-getting-started.html

This document describes how to install and configure ActiveMQ for both Unix and Windows' platforms.

3. http://www.apache.org/licenses/LICENSE-2.0.html

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

