# 15. Packaging and deployment

[Back to notes index](../README.md)

| [Previous: Scheduled and background work](../chapters/14-scheduled-and-background-work.md) | [Notes index](../README.md) | [Next: All code samples](../chapters/98-all-code-samples.md) |
|:--|:--:|--:|

## What I am learning here

A deployable Spring Boot application can be packaged as an executable archive that includes its dependencies and embedded server. Build the artifact once, then configure each environment externally. This keeps the same application binary moving through test and production environments.

## Build and run the archive

~~~sh
./mvnw clean verify
./mvnw package
java -jar target/notes-service-0.0.1-SNAPSHOT.jar
~~~

The first command compiles the code and runs tests. The package command creates the executable archive. Use the actual artifact filename produced by the build.

A container can run the archive with a Java runtime image:

~~~dockerfile
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY target/notes-service.jar app.jar
USER 10001
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
~~~

A production image should use a maintained runtime image, run with a non-root user, and avoid copying credentials into image layers. Supply environment-specific configuration through the deployment platform. Configure health probes and allow enough graceful shutdown time for in-flight work to finish.

Review memory and CPU limits, logs, database connectivity, secrets, network access, and rollback steps before release. A successful local build does not prove the deployed service can reach its dependencies.

## Questions to review

1. What does the executable archive contain?
2. What does clean verify do?
3. Why build the artifact once for several environments?
4. Which filename should java -jar use?
5. Why run a container as a non-root user?
6. Where should production secrets come from?
7. What does graceful shutdown allow?
8. Which deployment checks are needed beyond a successful build?

| [Previous: Scheduled and background work](../chapters/14-scheduled-and-background-work.md) | [Notes index](../README.md) | [Next: All code samples](../chapters/98-all-code-samples.md) |
|:--|:--:|--:|

