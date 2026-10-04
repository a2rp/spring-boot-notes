# 14. Scheduled and background work

[Back to notes index](../README.md)

| [Previous: Actuator, health, and observability](../chapters/13-actuator-health-and-observability.md) | [Notes index](../README.md) | [Next: Packaging and deployment](../chapters/15-packaging-and-deployment.md) |
|:--|:--:|--:|

## What I am learning here

Some work runs on a schedule or should happen outside the request thread. Scheduling is useful for periodic tasks such as refreshing a cache. It does not provide a durable job queue by itself. Work that must survive process restarts or run across several instances needs persistent coordination.

## Schedule a small task

~~~java
@Configuration
@EnableScheduling
class SchedulingConfiguration {}

@Component
class CatalogRefreshJob {
    @Scheduled(fixedDelayString = "PT5M")
    void refreshCatalog() {
        System.out.println("Refresh catalog data");
    }
}
~~~

The delay begins after the previous run finishes. A fixed rate measures time between scheduled starts and may behave differently for slow jobs. Move substantial work into a dedicated service and make failures visible through logs and metrics.

An in-memory schedule runs inside each application instance. If the application has three replicas, the job may run three times. Use a distributed scheduler, database lock, or a proper queue when only one coordinated execution is required.

For background methods with @Async, enable async execution and provide an appropriate executor. Do not assume an exception from fire-and-forget work will be returned to the original HTTP request. Keep task status and failure handling explicit when a user depends on the result.

## Questions to review

1. What kind of work fits a scheduled task?
2. Does an in-memory schedule provide a durable queue?
3. When does fixedDelay measure its wait?
4. What does fixedRate measure?
5. Why can several application replicas repeat one scheduled task?
6. What can coordinate a single distributed job?
7. What does @Async change?
8. How should a user-visible background task report its status?

| [Previous: Actuator, health, and observability](../chapters/13-actuator-health-and-observability.md) | [Notes index](../README.md) | [Next: Packaging and deployment](../chapters/15-packaging-and-deployment.md) |
|:--|:--:|--:|

