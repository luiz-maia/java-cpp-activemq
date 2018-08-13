# java-cpp-activemq
Proof of Concept for a project including a Java web application housed in Tomcat interfacing through ActiveMQ with an application written in C++.

Environment:

Windows 7 <br/>
Java Runtime Enviroment 1.8.181 <br/>
Tomcat 9.0.10 <br/>
ActiveMQ 5.15.4 <br/>
ActiveMQ-CPP 3.9.4 based on the CMS 3.2 <br/>
APR 1.6.3 <br/>

Java web application built with JMS running on Tomcat <br/>
C++ application linked with ActiveMQ-CPP (the Apache CMS API library implementation) running on Windows <br/>

Step-by-step

1. Download and install JRE.

Check item 1 of References.

2. Download Tomcat and unzip it into a specific folder.

Check item 2 of References.

3. ****** Configure Tomcat to not pick up Jetty WebSocket ServerContainerInitializer class (See References section).
4. ****** Download ActiveMQ and unzip it into a specific folder.
5. ****** Run an ActiveMQ Broker.
5. Manually integrate Tomcat and ActiveMQ.

Note: This does allow for Topic, Queue, and ConnectionFactory injection but does not support transactional sending and delivery. You should go to Tomcat documentation and read <b>JNDI Resources HOW-TO</b>, especially part: <b>Configure Tomcat's Resource Factory</b>. ActiveMQ has ready JNDI resource factory for all its administered objects: ConnectionFactory and destinations.
You must provide it as a parameter factory for your resources:

<Context>
  <Resource name="jms/ConnectionFactory" auth="Container" type="org.apache.activemq.ActiveMQConnectionFactory" description="JMS Connection Factory" factory="org.apache.activemq.jndi.JNDIReferenceFactory" brokerURL="vm://localhost" brokerName="LocalActiveMQBroker"/>
</Context>

Tomcat provides a number of specific options for JNDI resources that cannot be specified in web.xml. These include closeMethod that enables faster cleaning-up of JNDI resources when a web application stops and singleton that controls whether or not a new instance of the resource is created for every JNDI lookup. To use these configuration options the resource must be specified in a web application's <Context> element or in the <GlobalNamingResources> element of $CATALINA_BASE/conf/server.xml.

6. Ddownload the ActiveMQ-CPP API Source and Build it.

On Windows, use Visual Studio Community to open the solution file <b>\vs2010-build\activemq-cpp.sln</b> and compile all the solution projects. Before you can do that however it will be necessary to handle the solution dependencies, i.e. Apache Portable Runtime (APR) and CPPUnit tools, install those and make sure the project points to them correctly.

6.a. Download CppUnit Source Code and Build it.

Check item 5 of References.

CppUnit can be compiled using Visual Studio IDE. The <b>\cppunit-1.12.1\src\CppUnitLibraries.sln</b> solution file is provided. This workspace exposes the entire list of working .dsp projects that are required for the complete CppUnit binary release. It includes dependencies between the projects to assure that they are built in the appropriate order but it is necessary to migrate the whole projects to the new Visual Studio version. It will be done automatically by the IDE and then you will need to retarteg the solution to the updated Windows SDK verstion. Change the SDK version in the project property pages or by right-clicking the solution and selecting "Retarget solution". After that build the solution as a whole.

6.b. Download the APR Source Code and Build it.

Check item 5 of References.

APR can be compiled using Visual C++'s graphical environment. To simplify this process, a complete Visual Studio workspace, apr-util/aprutil.dsw, is provided. This workspace exposes the entire list of working .dsp projects that are required for the complete APR binary release. It includes dependencies between the projects to assure that they are built in the appropriate order but it is necessary to migrate the whole projects to the new Visual Studio version. It will be done automatically by the IDE and then you will need to retarteg the solution to the updated Windows SDK verstion. Change the SDK version in the project property pages or by right-clicking the solution and selecting "Retarget solution". After that build the solution as a whole.

On Windows, use Visual Studio Community to open the solution file <b>\apr-1.6.3\apr.sln</b> and compile all the solution projects. Before you can do that however it will be necessary to handle the solution dependencies, i.e. Apache Portable Runtime (APR) and CPPUnit tools, install those and make sure the project points to them correctly.

6. Implement and build the CMS C++ code fragment using 

References:

1. http://www.oracle.com/technetwork/pt/java/javase/downloads/jre8-downloads-2133155.html

Java Runtime Environment, or JRE™.

2. https://tomcat.apache.org/index.html

Apache Tomcat Latest Releases.

3. http://activemq.apache.org/download.html

Apache ActiveMQ Latest Releases.

3. http://activemq.apache.org/tomcat.html

Configuration issues for Tomcat 7 and later.
Tomcat needs to be configured to ignore Jetty SCI annotations so that the Jetty WebSocket ServerContainerInitializer class is not inadvertently picked up by Tomcat. For more information on this problem see AMQ-6154 and https://wiki.apache.org/tomcat/HowTo/FasterStartUp and consult the Tomcat documentation for the version you are using to properly exclude the Jetty jar files from being scanned by Tomcat.

4. https://tomcat.apache.org/tomcat-9.0-doc/index.html

This is the top-level entry point of the documentation bundle for the Apache Tomcat Servlet/JSP container. Apache Tomcat version 9.0 implements the Servlet 4.0 and JavaServer Pages 2.3 specifications from the Java Community Process, and includes many additional features that make it a useful platform for developing and deploying web applications and web services.

4.a. https://tomcat.apache.org/tomcat-9.0-doc/jndi-resources-howto.html

JNDI Resources HOW-TO.

4.b. https://tomcat.apache.org/tomcat-9.0-doc/jndi-resources-howto.html#Adding_Custom_Resource_Factories

Adding Custom Resource Factories. You can write your own factory and integrate it into Tomcat, and then configure the use of this factory in the <Context> element for the web application.

3.a. https://issues.apache.org/jira/browse/AMQ-6154

BUG Resolved: ActiveMQ with websocket on Tomcat fails with NullPointerException at org.eclipse.jetty.websocket.jsr356.server.deploy.WebSocketServerContainerInitializer

3.b. https://wiki.apache.org/tomcat/HowTo/FasterStartUp

This section provides several recommendations on how to make your web application and Apache Tomcat as a whole to start up faster.

4. https://tomcat.apache.org/tomcat-9.0-doc/index.html

This is the top-level entry point of the documentation bundle for the Apache Tomcat Servlet/JSP container. Apache Tomcat version 9.0 implements the Servlet 4.0 and JavaServer Pages 2.3 specifications from the Java Community Process, and includes many additional features that make it a useful platform for developing and deploying web applications and web services.

4. http://activemq.apache.org/version-5-getting-started.html

This document describes how to install and configure ActiveMQ for both Unix and Windows' platforms.

4. http://activemq.apache.org/cross-language-clients.html

Apache ActiveMQ is a message broker written in Java with JMS, REST and WebSocket interfaces, however it supports protocols like AMQP, MQTT, OpenWire and STOMP that can be used by applications in different languages.

5. http://activemq.apache.org/cms/

CMS (stands for C++ Messaging Service) is a JMS-like API for C++ for interfacing with Message Brokers such as Apache ActiveMQ. CMS helps to make your C++ client code much neater and easier to follow. To get a better feel for CMS try the API Reference. ActiveMQ-CPP is a client only library, a message broker such as Apache ActiveMQ is still needed for your clients to communicate.

Our implementation of CMS is called ActiveMQ-CPP, which has an architecture that allows for pluggable transports and wire formats. Currently we support the OpenWire and Stomp protocols, both over TCP and SSL, we also now support a Failover Transport for more reliable client operation. In addition to CMS, ActiveMQ-CPP also provides a robust set of classes that support platform independent constructs such as threading, I/O, sockets, etc. You may find many of these utilities very useful, such as a Java like Thread class or the "synchronized" macro that let's you use a Java-like synchronization on any object that implements the activemq::concurrent::Synchronizable interface. ActiveMQ-CPP is released under the Apache 2.0 License.

5.a. http://activemq.apache.org/cms/download.html

Apache ActiveMQ-CPP Latest Releases.

5.b. http://activemq.apache.org/cms/building.html

ApacheMQ-CPP Building Steps.

5.c. http://apr.apache.org/ 

The mission of the Apache Portable Runtime (APR) project is to create and maintain software libraries that provide a predictable and consistent interface to underlying platform-specific implementations.

5.c.1. https://sourceforge.net/projects/cppunit/

CppUnit - C++ port of JUnit. This is the C++ port of the famous JUnit framework for unit testing.

5.c.2. http://apr.apache.org/compiling_win32.html

APR can be built on Windows using a cmake-based build system or with Visual Studio project files maintained by APR developers. The cmake-based build system directly supports more versions of Visual Studio but currently has considerable functional limitations.



http://activemq.apache.org/run-broker.html

3. http://www.apache.org/licenses/LICENSE-2.0.html

TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

