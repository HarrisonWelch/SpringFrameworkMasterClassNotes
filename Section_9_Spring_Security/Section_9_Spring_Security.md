# Section 9 Spring Security

* Very important
* User has to be authenticated and authorized
* Register user to system.
* How to let a user into your system
* OAuth open ID
* Create User with REST API
* Confirmation email after signing
* Forgot password API - Email setup token
* OAuth 2 and OpenID - Seend with "Login with Google" or "Login with FB"

Create the spring-security-tutorial in the IntelliJ idea.

Make the client with https://start.spring.io/

Click this link to auto-import: https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.1.3&packaging=jar&jvmVersion=11&groupId=com.harrison&artifactId=spring-security-client&name=spring-security-client&description=Demo%20project%20for%20Spring%20Boot&packageName=com.harrison.client&dependencies=web,lombok,data-jpa,mysql

Table if the link expires:
| Property      | Setting                                |
| ------------- | -------------------------------------- |
| Project       | Maven                                  |
| Language      | Java                                   |
| Spring Boot   | 3.1.3 (latest non-snapshot and non-M*) |
| Group         | com.name                               |
| Artifact      | spring-security-client                 |
| Name          | spring-security-client                 |
| Description   | Demo project for Spring Boot           |
| Package name  | com.harrison.client                    |
| Packing       | Jar                                    |
| Java          | 11                                     |

Then we are going to add
* Spring Data JPA
* Lombok
* MySQL Driver
* Spring Web

Generate and extract that into

In the outer project adjust the pom.xml to add this:
```xml
  <packaging>pom</packaging>

  <modules>
    <module>spring-security-client</module>
  </modules>
```

Now dig into the client project that we just labeled as a module to the outer project

Rename the application.properties to applcation.yml. Then put this in there:

```yml
Server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/user_registration
    username: root
    password: Bingbong123$
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    show-sql: true
    hibernate:
      ddl-auto: update

```

Now create that schema in MySQL workbench

![sec_make_schema screenshot](https://github.com/HarrisonWelch/LearnSpring/blob/main/Screenshots/sec_make_schema.png)

Now build the following packages:
1. controller
2. config
3. entity
4. model
5. repository
6. service

![sec_pkg_struct screenshot](https://github.com/HarrisonWelch/LearnSpring/blob/main/Screenshots/sec_pkg_struct.png)

## Create Basic structure

RegistrationController.java
```java
package com.harrison.client.controller;

import com.harrison.client.entity.User;
import com.harrison.client.model.UserModel;
import com.harrison.client.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class RegistrationController {

    @Autowired
    private UserService userService;

    @PostMapping("/register")
    public String registerUser(@RequestBody UserModel userModel) {
        User user = userService.registerUser(userModel);
        return null;
    }
}
```

UserRepository.java
```java
package com.harrison.client.repository;

import com.harrison.client.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {

}
```

UserService.java
```java
package com.harrison.client.service;

import com.harrison.client.entity.User;
import com.harrison.client.model.UserModel;

public interface UserService {
    User registerUser(UserModel userModel);
}
```

UserServiceImpl:
```java
package com.harrison.client.service;

import com.harrison.client.entity.User;
import com.harrison.client.model.UserModel;
import com.harrison.client.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserRepository userRepository;

    @Override
    public User registerUser(UserModel userModel) {
        User user = new User();
        user.setEmail(userModel.getEmail());
        user.setFirstName(userModel.getFirstName());
        user.setLastName(userModel.getLastName());
        user.setRole("USER");
        user.setPassword(userModel.getPassword());

        return null;
    }
}
```

User.java:
```java
package com.harrison.client.entity;

import jakarta.persistence.*;
import lombok.Data;

@Entity
@Data
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String firstName;
    private String lastName;
    private String email;
    @Column(length = 60)
    private String password; // Will be encrypted later
    private String role;
    private boolean enabled = false; // default behavior
}
```

UserModel.java:
```java
package com.harrison.client.model;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class UserModel {
    private String firstName;
    private String lastName;
    private String email;
    private String password;
    private String matchingPassword;
}
```

Now for the security part

WebSecurityConfig.java (start of it)
```java
package com.harrison.client.config;

import org.springframework.context.annotation.Configuration;

@Configuration
public class WebSecurityConfig {
}

```

Now we go back to the https://start.spring.io/ and enter the security and open the Explore tab at the bottom. From there, this is what you want to put in the pom.xml (inner project for the client):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.security</groupId>
    <artifactId>spring-security-test</artifactId>
    <scope>test</scope>
</dependency>
```