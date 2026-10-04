# 17. Complete questions and answers

[Back to notes index](../README.md)

| [Previous: All code samples](98-all-code-samples.md) | [Notes index](../README.md) | [Next: Notes index](../README.md) |
|:--|:--:|--:|

## 1. Spring Boot foundations and first application

**1. What does Spring Boot configure for a common application?**

Spring Boot configures common infrastructure and dependencies from the application classpath and settings.

**2. Which part of the stack provides the core dependency injection container?**

Spring Framework provides the core container and dependency injection features.

**3. What does Spring Initializr create?**

Spring Initializr generates a project structure and selected build dependencies.

**4. What is the current Java minimum for Spring Boot 4.1.1?**

The current Boot 4.1.1 minimum is Java 17.

**5. What does @SpringBootApplication combine?**

@SpringBootApplication combines configuration, component scanning, and auto-configuration.

**6. What does SpringApplication.run start?**

SpringApplication.run creates the application context and starts the embedded web server when configured.

**7. Which starter adds Spring MVC and an embedded server in Boot 4?**

spring-boot-starter-webmvc adds Spring MVC and its server support in Boot 4.

**8. Why does the Maven parent manage dependency versions?**

The parent keeps compatible library versions aligned without repeating versions in the POM.

## 2. Project structure and configuration

**9. Where does main Java source usually live?**

Application Java source usually lives under src/main/java.

**10. Where are application properties stored?**

Application properties usually live under src/main/resources.

**11. What does the Maven wrapper provide?**

The wrapper runs the project's expected Maven version without requiring a global install.

**12. How can deployment configuration override a default property?**

A deployment can supply a higher-priority environment or external property value.

**13. Why should secrets stay out of committed configuration?**

Committed secrets can be exposed to anyone with repository access and should be rotated if leaked.

**14. What does @ConfigurationProperties bind?**

@ConfigurationProperties binds a group of named settings to a typed object.

**15. How can the dev profile be activated?**

Set SPRING_PROFILES_ACTIVE=dev or use the equivalent runtime configuration.

**16. What should a profile represent?**

A profile should represent environment-specific settings, not a separate copy of the app.

## 3. Beans and dependency injection

**17. What is a Spring bean?**

A Spring bean is an object registered and managed by the application context.

**18. What does dependency injection do?**

Dependency injection supplies an object's collaborators from the container.

**19. Why is constructor injection useful?**

Constructor injection makes requirements visible and supports plain unit testing.

**20. Why can a required dependency be final?**

A final field prevents replacing a required collaborator after construction.

**21. Is @Autowired required on a single constructor?**

No, Spring can use a class's single constructor without @Autowired.

**22. When is @Bean useful?**

@Bean creates a managed object when a library class cannot be annotated directly.

**23. What does @Repository communicate?**

@Repository marks a data-access component and participates in persistence exception translation.

**24. What should I do when several beans implement one interface?**

Use a qualifier or another explicit selection rule when several implementations exist.

## 4. REST controllers and HTTP requests

**25. What does @RestController add?**

@RestController makes a class a detected controller whose return values are written to the response body.

**26. What does GET usually do?**

GET normally reads a resource or collection without changing server state.

**27. Which HTTP method usually creates a resource?**

POST commonly creates a resource or starts a server-side operation.

**28. What does @PathVariable read?**

@PathVariable reads a value embedded in the route path.

**29. What does @RequestParam read?**

@RequestParam reads a query parameter or request parameter.

**30. Which application layer should hold business decisions?**

A service should own business decisions and use-case coordination.

**31. What status does ResponseEntity.created use?**

ResponseEntity.created returns HTTP 201 Created.

**32. Why should a controller accept a request DTO?**

A request DTO limits the accepted fields and protects the persistence model boundary.

## 5. DTOs and request validation

**33. What does DTO stand for?**

DTO means Data Transfer Object.

**34. Why separate request DTOs from database entities?**

Separate DTOs keep the API contract independent from persistence details.

**35. What does @RequestBody do?**

@RequestBody deserializes the request body, commonly JSON, into a Java object.

**36. What does @Valid trigger?**

@Valid asks the validation provider to check the declared constraints.

**37. What does @NotBlank reject?**

@NotBlank rejects null, empty, and whitespace-only text.

**38. Which response status is common for invalid request data?**

400 Bad Request is the common status for invalid request data.

**39. Where should a duplicate-email business rule be checked?**

Check uniqueness in the service/domain operation and enforce it in the database too.

**40. Why return a response DTO?**

A response DTO exposes only the fields the client is allowed to receive.

## 6. Error handling and API responses

**41. Why centralize API error handling?**

Central handling avoids repeating exception-to-response mapping in each controller.

**42. What does @RestControllerAdvice apply to?**

@RestControllerAdvice applies exception handling across controller responses.

**43. Which status represents a missing resource?**

404 Not Found represents a missing requested resource.

**44. What does ProblemDetail contain?**

ProblemDetail carries an HTTP status and descriptive fields such as title and detail.

**45. Should a response reveal database credentials or stack traces?**

No. Internal stack traces and database details should remain server-side.

**46. What is the difference between 401 and 403?**

401 means valid authentication is absent; 403 means the authenticated caller lacks permission.

**47. Where should missing-resource conditions usually be detected?**

Detect absence in the service or repository-backed use case and translate it to an API error.

**48. How should an unexpected server failure be shown to a client?**

Return a generic safe message and log enough context for investigation.

## 7. Persistence with Spring Data JPA

**49. What does JPA map?**

JPA maps Java entity objects to relational database tables and rows.

**50. What does @Entity mark?**

@Entity marks a class as a persistence entity.

**51. What does @Id identify?**

@Id identifies an entity's primary key.

**52. Why does a JPA entity need an identifier?**

JPA needs an identifier to distinguish and track persisted entities.

**53. What implementation does Spring Data create?**

Spring Data generates a repository implementation from the interface definition.

**54. What do JpaRepository's type parameters represent?**

The type parameters represent the entity class and its ID type.

**55. What can a derived query method do?**

A derived method name can describe a common query based on entity properties.

**56. Why keep JPA entities out of API responses?**

Entities may expose internal fields or lazy relations, so map them to response DTOs.

## 8. Service layer and transactions

**57. What belongs in a service layer?**

A service coordinates a use case and business rules.

**58. What does a repository handle?**

A repository provides persistence operations and queries.

**59. What is a database transaction?**

A transaction groups database changes into one commit or rollback unit.

**60. Why group related writes into one transaction?**

Grouping related writes prevents partial database state when a step fails.

**61. What does readOnly = true communicate?**

readOnly=true marks a query-oriented operation and may allow provider optimizations.

**62. Does one database transaction automatically cover a remote HTTP call?**

No. A local database transaction does not atomically include a remote HTTP call.

**63. Which method is a clear place for a transaction boundary?**

A public service method called through a Spring bean boundary is a clear starting point.

**64. Why test rollback behavior for important updates?**

Test that failed operations roll back changes which must remain atomic.

## 9. Database migrations and configuration

**65. Why externalize database settings?**

External configuration lets one artifact use environment-specific URLs and credentials.

**66. Where should production credentials be stored?**

Store production credentials in a protected secret manager or deployment environment.

**67. What does a database migration record?**

A migration records one ordered database schema change.

**68. What does Flyway apply?**

Flyway discovers and applies versioned migrations to the configured database.

**69. What does ddl-auto=validate do?**

ddl-auto=validate checks that the schema matches the mapped entities without creating it.

**70. Why avoid automatic schema creation in production?**

Production schema changes should be explicit and reviewed rather than silently generated by Hibernate.

**71. What does the V1__ prefix mean in a Flyway filename?**

V1 marks the first migration version in the filename convention.

**72. Why should an applied migration remain unchanged?**

Changing an applied migration breaks the recorded history and can make environments diverge.

## 10. Security and authorization

**73. What does authentication establish?**

Authentication establishes the identity of the caller.

**74. What does authorization decide?**

Authorization decides whether that caller may perform an operation.

**75. Where does Spring Security apply its filters?**

Spring Security applies request filters before controller handling.

**76. What happens when the security starter is added without custom rules?**

The starter applies default security so requests require authentication until rules are configured.

**77. What does permitAll mean for a matched route?**

permitAll allows the matched route without authentication.

**78. Why is HTTP Basic only a simple example here?**

HTTP Basic demonstrates authentication simply but may not fit a production client or deployment.

**79. When might service-method authorization help?**

Service-method authorization helps enforce an operation rule regardless of which controller calls it.

**80. Why should CSRF protection not be disabled without a reason?**

CSRF protects browser requests that automatically attach credentials, so disable it only after assessing the authentication design.

## 11. Testing Spring applications

**81. What does a plain unit test isolate?**

A unit test checks a small behavior without loading the whole application.

**82. What is a test slice?**

A slice test loads a selected application boundary and its required infrastructure.

**83. What does @WebMvcTest focus on?**

@WebMvcTest focuses on the Spring MVC controller layer.

**84. What does MockMvc simulate?**

MockMvc performs simulated servlet requests without opening a real network server.

**85. Why replace a service with a test double in an MVC test?**

A test double isolates HTTP mapping from the service and database behavior.

**86. When is a full application test useful?**

Use a full context test for application wiring, configuration, or startup behavior.

**87. What should a test assert?**

Tests should assert observable outputs and important side effects.

**88. Why keep test data small and descriptive?**

Small named test data makes failure messages easier to understand.

## 12. HTTP clients and resilience

**89. Why keep an HTTP integration behind a client class?**

An HTTP client class centralizes URL construction, transport behavior, and response mapping.

**90. What does RestClient provide?**

RestClient is Spring Framework's synchronous fluent HTTP client.

**91. Why set connection and response timeouts?**

Timeouts bound how long an upstream call can occupy application resources.

**92. Which value should be external configuration?**

Externalize the upstream base URL for each deployment environment.

**93. What should happen for an upstream error response?**

Translate transport and non-success responses into application outcomes callers can handle.

**94. When is retry reasonable?**

Retry a transient failure only when repeating the operation is safe.

**95. Why can retrying a POST duplicate work?**

A timed-out POST may have succeeded remotely, so retry can duplicate a side effect.

**96. What can make a write operation safe to repeat?**

An idempotency key lets the server recognize and safely repeat the same logical request.

## 13. Actuator, health, and observability

**97. What does Actuator add?**

Actuator provides management endpoints and operational integrations.

**98. What does a health endpoint help an operator inspect?**

A health endpoint exposes whether configured parts of the service are healthy.

**99. What is the difference between logs and metrics?**

Logs record events; metrics summarize measurements over time.

**100. What do traces connect?**

Traces connect operations across application and service boundaries.

**101. Why expose only selected management endpoints?**

Explicit exposure reduces accidental information disclosure and attack surface.

**102. Why protect detailed health output?**

Detailed health can reveal internal dependencies and should require authorization.

**103. What does liveness ask?**

Liveness asks whether the process should be restarted.

**104. What does readiness ask?**

Readiness asks whether the instance should receive new traffic.

## 14. Scheduled and background work

**105. What kind of work fits a scheduled task?**

Periodic refresh or cleanup can fit a scheduled task.

**106. Does an in-memory schedule provide a durable queue?**

No. In-memory schedules do not persist jobs or coordinate delivery across restarts.

**107. When does fixedDelay measure its wait?**

fixedDelay measures from the end of one run to the start of the next.

**108. What does fixedRate measure?**

fixedRate measures scheduled time between task starts.

**109. Why can several application replicas repeat one scheduled task?**

Each replica owns its scheduler and may run the same task independently.

**110. What can coordinate a single distributed job?**

A distributed scheduler, database lock, or durable queue can coordinate the work.

**111. What does @Async change?**

@Async runs work through an asynchronous executor instead of waiting in the caller flow.

**112. How should a user-visible background task report its status?**

Store explicit task state and expose progress or failure when the user depends on completion.

## 15. Packaging and deployment

**113. What does the executable archive contain?**

An executable Boot archive packages application classes, dependencies, and a launchable server setup.

**114. What does clean verify do?**

clean verify removes previous build output, compiles, and runs verification including tests.

**115. Why build the artifact once for several environments?**

Building once avoids environment-specific recompilation differences between promotion stages.

**116. Which filename should java -jar use?**

Use the exact archive name printed in the build output.

**117. Why run a container as a non-root user?**

A non-root user limits the container process permissions if it is compromised.

**118. Where should production secrets come from?**

Supply secrets through the deployment platform or a secret manager.

**119. What does graceful shutdown allow?**

Graceful shutdown gives in-flight requests time to finish before process exit.

**120. Which deployment checks are needed beyond a successful build?**

Check health probes, database access, secrets, logs, resource limits, and rollback behavior.

## Cross-topic review

**121. What is the request path through a typical layered web application?**

The controller maps HTTP input, the service applies the use case, and the repository reads or writes persistent data.

**122. Where should request validation happen?**

Validate structural input at the API boundary, then enforce business and database invariants in their owning layers.

**123. Why use a response DTO with a JPA entity?**

It separates the public response fields from persistence shape and lazy-loading behavior.

**124. What should happen to secrets in Git history?**

Remove exposure and rotate the secret because deleting a file alone does not invalidate an exposed credential.

**125. What makes an API error useful but safe?**

A relevant status and concise next-step message without internal traces or sensitive details.

**126. Which layer should own retries for an upstream service?**

The client or resilience policy around that integration should own bounded retries, based on idempotency and failure type.

**127. Why are readiness and liveness separate?**

A process can be alive but unable to serve traffic while a dependency is unavailable.

**128. What does a deployment artifact represent?**

A built version of the application that is configured for its target environment at runtime.

| [Previous: All code samples](98-all-code-samples.md) | [Notes index](../README.md) | [Next: Notes index](../README.md) |
|:--|:--:|--:|
