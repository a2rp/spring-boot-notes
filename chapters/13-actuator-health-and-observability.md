# 13. Actuator, health, and observability

[Back to notes index](../README.md)

| [Previous: HTTP clients and resilience](../chapters/12-http-clients-and-resilience.md) | [Notes index](../README.md) | [Next: Scheduled and background work](../chapters/14-scheduled-and-background-work.md) |
|:--|:--:|--:|

## What I am learning here

Production operation needs evidence about whether a service is running and what it is doing. Spring Boot Actuator provides management endpoints for health, metrics, and other operational details. Logs record discrete events; metrics summarize measurements over time; traces connect work across service boundaries.

## Expose only the endpoints the team needs

~~~properties
spring.application.name=notes-service
management.endpoints.web.exposure.include=health,info
management.endpoint.health.show-details=when-authorized
~~~

Keep management endpoints protected and expose only the ones that are needed. Do not publish environment variables, configuration values, or detailed health data to anonymous visitors.

Use a stable application name so logs and metrics can be identified. Prefer structured messages that include a useful request or correlation ID, but avoid logging passwords, access tokens, personal data, or full request bodies by default.

Health checks have different purposes. Liveness asks whether the process should be restarted. Readiness asks whether it can receive traffic. A database outage may make a service unready without meaning the process itself is dead. Check the deployment platform's probe behavior before wiring dependencies into each health signal.

## Questions to review

1. What does Actuator add?
2. What does a health endpoint help an operator inspect?
3. What is the difference between logs and metrics?
4. What do traces connect?
5. Why expose only selected management endpoints?
6. Why protect detailed health output?
7. What does liveness ask?
8. What does readiness ask?

| [Previous: HTTP clients and resilience](../chapters/12-http-clients-and-resilience.md) | [Notes index](../README.md) | [Next: Scheduled and background work](../chapters/14-scheduled-and-background-work.md) |
|:--|:--:|--:|

