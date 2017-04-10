Spring Cloud Microservice App
=============================

 

![Demo System Schematic](https://github.com/paulc4/microservices-demo/blob/master/mini-system.jpg)

 

![](shopping-system.jpg)

 

![](mini-system.jpg)

 

Run Microservice App in IDE
---------------------------

You can run the system in your IDE by running the three servers in order:

*1. ***RegistrationService.java** -\> **Eureka GUI: http://localhost:1111**

a) \@EnableEurekaServer
(org.springframework.cloud.netflix.eureka.server.EnableEurekaServer)

b) use registration-server.yml/properties

\*Note: Spring Cloud also supports [Consul](https://www.consul.io) as an
alternative to Eureka.

Support REST API:

http://localhost:1111/eureka/apps/

 

*2. ***AccountsServer.java (Domain Service) -\> Service GUI
http://localhost:2222**

\@EnableDiscoveryClient   -\> Eureka service: **ACCOUNTS-SERVICE**

\@Import(AccountsConfiguration.class)

use accounts-server.properties/yml

Support REST API:

http://localhost:2222/accounts/123456789

\* Note: Apply Loose Coupling and Tight Cohesion principle to interacting
processes.

\* **Note:** The Accounts microservice provides a RESTful interface over HTTP,
but any suitable protocol could be used. Messaging using
[AMQP](http://rabbitmq.docs.pivotal.io) or JMS is an obvious alternative.

 

Via Service Discovery by RestTemplate

http://ACCOUNTS-SERVICE/accounts/123456789

\$ curl http://ACCOUNTS-SERVICE/accounts/123456789  OR browser won’t know Eureka
Service

curl: (6) Couldn't resolve host 'ACCOUNTS-SERVICE'

 

*3. ***WebServer.java (Orchestration Layer) -\> Web App GUI
http://localhost:3333**

\@EnableDiscoveryClient   -\> Eureka Service: **WEB-SERVICE**

\@ComponentScan(useDefaultFilters = false) // Disable component scanner

use web-server.properties/yml

\@LoadBalanced  - takes the logical service-name (as registered with the
*discovery-server*) and converts it to the actual hostname of the chosen
microservice.

Support GUI Access:

http://localhost:3333  -- Landing page by a Spring MVC Controller

http://localhost:3333/accounts/search

http://localhost:3333/accounts/123456789

http://localhost:3333/accounts/owner/Keri

 

 

 

Run in Command Line
-------------------

1.  Window1, `mvn clean package`

2.  Window1: `java -jar target/microservice-demo-1.1.0.RELEASE.jar registration`

3.  Window2: `java -jar target/microservice-demo-1.1.0.RELEASE.jar accounts`

4.  Window3: `java -jar target/microservice-demo-1.1.0.RELEASE.jar web`

5.  In browser access two links:

    <Eureka UI: http://localhost:1111>

    WebApp UI:  <http://localhost:3333>

 

 

1.  Window4: run up another **account-server** using HTTP port 2223:

    -   `java -jar target/microservice-demo-0.0.1-SNAPSHOT.jar accounts 2223`

2.  Allow it to register itself

    <Check in Eureka UI: http://localhost:1111>

3.  Kill the first account-server and see the web-server switch to using the new
    account-server - no loss of service.

 

https://spring.io/blog/2015/07/14/microservices-with-spring
