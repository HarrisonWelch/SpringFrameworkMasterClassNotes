# Spring Boot

* Basics of the theory
* What is Spring and what is SpringBoot
* How to create a REST API using SpringBoot
* Add exception handling
* Implement unit testing for all layers
* Database config
  * H2 into SQL
* How to monitor application
* How to deploy the application into production

## What is Spring

* Framework to create enterprise application
* There are a lot more configs and tech
* Allows us to do many things
  * SpringWeb, SpringJPA, etc.
  * Hibernate
* A lot of moving parts, so Spring makes it easy
* Conventrate on convention not configuration

## What is spring framework
* Extension layer on to the Spring
* Rapid application development
* Managing dependencies
  * Group all dependencies into a starter template
* Spring testing like JUnit and Mockito
* Auto configuration
* Springboot does the auto config for all those libraries
* We create jar file with a server embedded. Always production ready.
* Microservices make spring boot the way to go.

## Dependency Injection
* Default way for creating objects
* Student s = new Student()
* Inversion of control (IOC) Nothing but to give control from yourself to Spring to create the object itself.
* Spring will create the objects for you.
* Stored in one beans particular container

## Generate the project

start.spring.io

Share code: https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.1.3&packaging=jar&jvmVersion=11&groupId=com.harrison&artifactId=Spring-boot-tutorial&name=Spring-boot-tutorial&description=Demo%20project%20for%20Spring%20Boot&packageName=com.harrison.Springboot.tutorial&dependencies=web,h2

Settings are 
* Project: Maven
* Language: Java
* Spring Boot: 3.1.3 (latest stable (non-SNAPSHOT/M2/etc.))
* Group: com.harrison
* Artifact: Spring-boot-tutorial
* Name: Spring-boot-tutorial
* Description: Demo project for Spring Boot
* Package name: com.harrison.Springboot.tutorial
* Packaging: Jar
* Java: 11

Click "Explore" to preview

Click "Generate" to download the project template

Can open in IntelliJ or STS4 - Spring tool suite 4.

Explore the pom.xml, many dependencies and those have many dependencies.

## The @SpringBootApplication annotation
* Configuration setup. Set for all packages and sub-pckgs.

Run the application
* Note: I had to update my java to 17 (amzn corretto) eventhough I selected java 11 in the generator :)

output
```

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v3.1.3)

2023-08-27T15:34:44.476-04:00  INFO 20724 --- [           main] c.h.S.t.SpringBootTutorialApplication    : Starting SpringBootTutorialApplication using Java 17.0.8.1 with PID 20724 (C:\Users\harri\OneDrive\Desktop\Code\LearnSpring\Section_7_Spring_Boot\Spring-boot-tutorial\target\classes started by harri in C:\Users\harri\OneDrive\Desktop\Code\LearnSpring\Section_7_Spring_Boot\Spring-boot-tutorial)
2023-08-27T15:34:44.480-04:00  INFO 20724 --- [           main] c.h.S.t.SpringBootTutorialApplication    : No active profile set, falling back to 1 default profile: "default"
2023-08-27T15:34:44.987-04:00  INFO 20724 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2023-08-27T15:34:44.992-04:00  INFO 20724 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2023-08-27T15:34:44.992-04:00  INFO 20724 --- [           main] o.apache.catalina.core.StandardEngine    : Starting Servlet engine: [Apache Tomcat/10.1.12]
2023-08-27T15:34:45.050-04:00  INFO 20724 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2023-08-27T15:34:45.052-04:00  INFO 20724 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 540 ms
2023-08-27T15:34:45.306-04:00  INFO 20724 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2023-08-27T15:34:45.314-04:00  INFO 20724 --- [           main] c.h.S.t.SpringBootTutorialApplication    : Started SpringBootTutorialApplication in 1.039 seconds (process running for 1.272)
```

Tomcat is running on 8080

Loading http://localhost:8080/ will bring back some things, but it will error with blank fallback.

```
Whitelabel Error Page
This application has no explicit mapping for /error, so you are seeing this as a fallback.

Sun Aug 27 15:42:02 EDT 2023
There was an unexpected error (type=Not Found, status=404).
```

## HelloController

```java
package com.harrison.Springboot.tutorial.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

@RestController // Component of a particular type, component is added by this annotation
public class HelloController {

    @RequestMapping(value = "/", method = RequestMethod.GET) // Assign default endpoint to / as a GET
    public String helloWorld() {
        return "Welcome too Daily Code Buffer!!";
    }
}

```

Reloading and going into localhost:8080 shows the "Welcome too Daily Code Buffer!!" in basic HTML

## Application.properties

Change the default port away from 8080 for example to 8082.

```shell
server.port = 8082
# Documentation available for each property
```

## Run it

Go to http://localhost:8082/
-> you get "Welcome too Daily Code Buffer!!"

## Now

The request mapping is still verbose