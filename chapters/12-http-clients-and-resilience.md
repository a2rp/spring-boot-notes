# 12. HTTP clients and resilience

[Back to notes index](../README.md)

| [Previous: Testing Spring applications](../chapters/11-testing-spring-applications.md) | [Notes index](../README.md) | [Next: Actuator, health, and observability](../chapters/13-actuator-health-and-observability.md) |
|:--|:--:|--:|

## What I am learning here

A server-side application often calls another HTTP service. Keep that integration behind a client class so controllers and business services do not need to construct URLs or parse transport details. Set connection and response timeouts so a slow dependency cannot occupy application resources forever.

Spring Framework provides RestClient for synchronous calls. Spring Boot can provide a configured builder when the relevant web dependencies are present.

## Isolate an outbound client

~~~java
@Configuration
class CatalogClientConfiguration {
    @Bean
    RestClient catalogRestClient(RestClient.Builder builder) {
        return builder
            .baseUrl("https://catalog.example.test")
            .build();
    }
}
~~~

~~~java
@Component
class CatalogClient {
    private final RestClient restClient;

    CatalogClient(RestClient catalogRestClient) {
        this.restClient = catalogRestClient;
    }

    CatalogItem findById(long id) {
        return restClient.get()
            .uri("/api/items/{id}", id)
            .retrieve()
            .body(CatalogItem.class);
    }
}
~~~

The example domain uses the reserved .test suffix and is a placeholder. Configure a real service URL outside the source code. Translate connection failures and non-success responses into outcomes that the service layer can handle.

Retries, timeouts, and circuit breakers address different failure modes. Retry only when the operation is safe to repeat and the failure may be temporary. A timed-out POST may have succeeded remotely even when the caller did not receive the response. Make write operations idempotent or use a request key before retrying them.

## Questions to review

1. Why keep an HTTP integration behind a client class?
2. What does RestClient provide?
3. Why set connection and response timeouts?
4. Which value should be external configuration?
5. What should happen for an upstream error response?
6. When is retry reasonable?
7. Why can retrying a POST duplicate work?
8. What can make a write operation safe to repeat?

| [Previous: Testing Spring applications](../chapters/11-testing-spring-applications.md) | [Notes index](../README.md) | [Next: Actuator, health, and observability](../chapters/13-actuator-health-and-observability.md) |
|:--|:--:|--:|

