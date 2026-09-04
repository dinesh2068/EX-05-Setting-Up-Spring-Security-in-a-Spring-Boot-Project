# EXP05 Setting Up Spring Security in a Spring Boot Project

## AIM

To write a program for setting up **Spring Security** in a Spring Boot
project to secure endpoints using **Basic Authentication** and
role-based access control.


## ALGORITHM

1.  Create a Spring Boot project using Maven.
2.  Add the required dependencies:
    -   Spring Web
    -   Spring Security
    -   Spring Boot DevTools (optional)
3.  Configure Spring Security using a `SecurityConfig` class.
4.  Create an in-memory user with:
    -   Username: `user`
    -   Password: `password`
    -   Role: `USER`
5.  Configure the security filter to allow the `/public` endpoint
    without authentication.
6.  Configure all other endpoints to require authentication.
7.  Enable HTTP Basic Authentication.
8.  Create a REST controller with public and secured endpoints.
9.  Run the Spring Boot application.
10. Test the endpoints using a browser or Postman.
11. Verify that the public endpoint is accessible without login and the
    private endpoint requires username and password.


# PROGRAM

## 1. `pom.xml`

``` 
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>

</dependencies>
```


## 2. `SecurityConfig.java`

``` 
package com.example.demo;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.provisioning.UserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/public").permitAll()
                .anyRequest().authenticated()
            )
            .httpBasic(httpBasic -> {});

        return http.build();
    }

    @Bean
    public UserDetailsManager userDetailsService() {

        UserDetails user = User.withDefaultPasswordEncoder()
                .username("user")
                .password("password")
                .roles("USER")
                .build();

        return new InMemoryUserDetailsManager(user);
    }
}
```


## 3. `HelloController.java`

``` 
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/public")
    public String publicEndpoint() {
        return "This is a public endpoint.";
    }

    @GetMapping("/private")
    public String privateEndpoint() {
        return "This is a secured endpoint. You are authenticated!";
    }
}
```

## 4. `DemoApplication.java`

```
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}

```


# OUTPUT



# RESULT

Thus, Spring Security was successfully configured in the Spring Boot application. The public endpoint was made accessible without
authentication, while the private endpoint was secured using Basic Authentication with an in-memory user having the USER role.
