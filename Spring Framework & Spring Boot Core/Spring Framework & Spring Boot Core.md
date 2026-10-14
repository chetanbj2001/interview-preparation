# Spring Framework and Spring Boot Core

## 1. Overview: Spring vs Spring Boot
* **Spring Framework**: A comprehensive programming and configuration model for modern Java-based enterprise applications. It focuses on Dependency Injection (DI) and Inversion of Control (IoC). However, it requires extensive boilerplate XML or Java configuration and manual setup of dependencies/servers.
* **Spring Boot**: An opinionated extension of the Spring Framework that eliminates boilerplate configuration, embeds a servlet container (Tomcat/Jetty), provides "starter" dependencies, and enables auto-configuration to get production-ready apps up and running quickly.

---

## 2. Inversion of Control (IoC) and Dependency Injection (DI)
* **IoC (Inversion of Control)**: A design principle where the control of object creation, configuration, and lifecycle is transferred from the application code to a container (the Spring IoC Container).
* **Dependency Injection (DI)**: The mechanism by which the Spring container injects dependent objects (beans) into a class rather than the class instantiating them directly using `new`.

### Types of Dependency Injection
1. **Constructor Injection (Recommended)**: Dependencies are passed via the class constructor. Makes immutability possible (`final` fields) and guarantees required dependencies are present before bean initialization.
2. **Setter Injection**: Dependencies are set via setter methods. Useful for optional dependencies or circular dependencies.
3. **Field Injection (`@Autowired` on fields)**: Injected directly via reflection. Not recommended because it hinders unit testing and immutability.

```java
import org.springframework.stereotype.Component;

@Component
public class OrderService {
    
    private final PaymentRepository paymentRepository;

    // Constructor Injection (Preferred)
    public OrderService(PaymentRepository paymentRepository) {
        this.paymentRepository = paymentRepository;
    }

    public void processOrder() {
        paymentRepository.process();
    }
}

@Component
class PaymentRepository {
    public void process() {
        System.out.println("Processing payment...");
    }
}
```

---

## 3. Spring Bean Scopes
Spring container manages the lifecycle of objects known as **Beans**. Bean scopes define how long a bean instance lives and how it is shared.

| Scope | Description |
| :--- | :--- |
| **`singleton` (Default)** | Only **one instance** per Spring IoC container. Shared globally across requests. |
| **`prototype`** | A **new instance** is created every time the bean is requested. |
| **`request`** | One bean instance per HTTP request (Web-aware application context only). |
| **`session`** | One bean instance per HTTP session (Web-aware application context only). |
| **`application`** | One bean instance per `ServletContext` lifecycle. |

---

## 4. Spring Bean Lifecycle
Understanding the sequence of events from bean instantiation to destruction is crucial for backend interviews.

1. **Instantiation**: Container finds bean definition and instantiates the object.
2. **Populate Properties**: Container injects dependencies and values.
3. **Aware Interfaces**: If implemented, calls methods like `BeanNameAware.setBeanName()`, `BeanFactoryAware.setBeanFactory()`.
4. **Post-Processing (Before Initialization)**: Invokes `BeanPostProcessor.postProcessBeforeInitialization()`.
5. **Initialization**: Calls custom init methods (e.g., `@PostConstruct` or `InitializingBean.afterPropertiesSet()`).
6. **Post-Processing (After Initialization)**: Invokes `BeanPostProcessor.postProcessAfterInitialization()`.
7. **Bean Ready for Use**: The bean resides in the container.
8. **Destruction**: When container shuts down, calls custom destruction methods (`@PreDestroy` or `DisposableBean.destroy()`).

---

## 5. Spring Boot Auto-Configuration & Starters

### Starters
Spring Boot Starters are a set of convenient dependency descriptors that you can include in your build. Instead of manually pulling down 10 different Hibernate/JDBC/Spring libraries, you include `spring-boot-starter-data-jpa`.

### Auto-Configuration (`@EnableAutoConfiguration`) & How It Works Under the Hood
Spring Boot automatically configures your application based on the jar dependencies present on the classpath. 
* It looks for configuration classes listed in `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` (in Spring Boot 3+) or `spring.factories` (in Spring Boot 2.x).
* It uses conditional annotations to apply configurations conditionally:
  * `@ConditionalOnClass`: Configures a bean only if a specific class is present on the classpath.
  * `@ConditionalOnMissingBean`: Configures a bean only if a bean of that type does not already exist.
  * `@ConditionalOnProperty`: Configures a bean based on whether a specific property exists and has a specific value.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// @SpringBootApplication combines @Configuration, @EnableAutoConfiguration, and @ComponentScan
@SpringBootApplication
public class Application {
    public staticmain(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

---

## 6. Profiles and Configuration Management
Profiles provide a way to segregate parts of your application configuration and make it available only in certain environments (e.g., `dev`, `qa`, `prod`).

```yaml
# application.yml example with profiles
spring:
  profiles:
    active: dev

---
spring:
  config:
    activate:
      on-profile: dev
server:
  port: 8080

---
spring:
  config:
    activate:
      on-profile: prod
server:
  port: 9090
```

---

## 7. Global Exception Handling in Spring Boot
Instead of handling exceptions in every controller using `try-catch`, Spring Boot provides `@RestControllerAdvice` to handle exceptions globally and return standardized error response payloads.

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;
import java.time.LocalDateTime;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage(),
            LocalDateTime.now()
        );
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);
    }
}

class ErrorResponse {
    private int status;
    private String message;
    private LocalDateTime timestamp;

    public ErrorResponse(int status, String message, LocalDateTime timestamp) {
        this.status = status;
        this.message = message;
        this.timestamp = timestamp;
    }
    // Getters
}

class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

---

## 8. Spring Boot Actuator
Spring Boot Actuator brings production-ready features to your application by exposing operational information over HTTP or JMX.

* **Common Endpoints**:
  * `/actuator/health`: Shows application health status (UP/DOWN) and health indicators (DB, disk space).
  * `/actuator/metrics`: Exposes JVM metrics, CPU usage, GC counters, and HTTP request statistics.
  * `/actuator/env`: Exposes configuration properties from the Spring `Environment`.
* **Security Note**: In production, sensitive Actuator endpoints should be secured using Spring Security to prevent unauthorized access.

---

## 9. Tricky Interview Questions & Answers

### Q1. What is the difference between `@Component`, `@Service`, and `@Repository`?
Functionally, they are identical—all mark a class as a Spring-managed bean via component scanning. However, they carry semantic meaning and special features:
* **`@Component`**: General-purpose stereotype annotation for any Spring-managed component.
* **`@Service`**: Indicates a business logic layer component.
* **`@Repository`**: Indicates a data access layer component. Crucially, it enables automatic **persistence exception translation** (translating vendor-specific database exceptions into Spring's unified `DataAccessException` hierarchy).

### Q2. What is circular dependency in Spring, and how does Spring handle it?
A circular dependency occurs when Bean A depends on Bean B, and Bean B depends on Bean A (or a cycle of multiple beans). 
* **Handling**: For **singleton** beans using **constructor injection**, Spring cannot resolve the cycle and throws a `BeanCurrentlyInCreationException` at startup. For **setter injection** or field injection, Spring resolves it using **tri-level caching** and early bean references.
* **Best Practice**: Refactor code to eliminate circular dependencies or use `@Lazy` injection.

### Q3. Why is Constructor Injection preferred over Field Injection?
1. **Immutability**: Dependencies can be marked `final`.
2. **Null Safety**: Guarantees dependencies are fully initialized before the bean is created.
3. **Easier Unit Testing**: You can instantiate classes with mock dependencies directly via `new Class(mockDep)` without needing a running Spring container.

### Q4. What happens under the hood when you annotate a class with `@SpringBootApplication`?
It is a meta-annotation comprising three primary annotations:
1. **`@Configuration`**: Tags the class as a source of bean definitions.
2. **`@EnableAutoConfiguration`**: Tells Spring Boot to start adding beans based on classpath settings via `AutoConfiguration.imports`.
3. **`@ComponentScan`**: Enables component scanning in the current package and sub-packages for `@Component`, `@Service`, etc.

---

## 10. Quick Revision
* **IoC / DI**: Spring container manages object lifecycles and injects dependencies.
* **Constructor Injection**: Recommended over field injection for testability and immutability.
* **Bean Scopes**: `singleton` (default, shared instance) vs `prototype` (new instance every request).
* **Auto-Configuration**: Powered by conditional annotations (`@ConditionalOnClass`, `@ConditionalOnMissingBean`) checking the classpath.
* **Global Exception Handling**: Centralized error mapping via `@RestControllerAdvice` and `@ExceptionHandler`.
* **Actuator**: Built-in production monitoring endpoints like `/actuator/health` and `/actuator/metrics`.
