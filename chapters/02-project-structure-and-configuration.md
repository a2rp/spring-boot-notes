# 2. Project structure and configuration

[Back to notes index](../README.md)

| [Previous: Spring Boot foundations and first application](../chapters/01-foundations-and-first-application.md) | [Notes index](../README.md) | [Next: Beans and dependency injection](../chapters/03-beans-and-dependency-injection.md) |
|:--|:--:|--:|

## What I am learning here

A Spring Boot project separates application source, tests, resources, and build configuration. Java code usually lives under src/main/java and tests under src/test/java. Configuration files and templates live under src/main/resources. The main application class should sit in a package above the application components so component scanning can find them.

## Recognize the project layout

~~~text
src/
├── main/
│   ├── java/com/example/notes/
│   │   └── NotesApplication.java
│   └── resources/
│       └── application.properties
└── test/
    └── java/com/example/notes/
pom.xml
mvnw
mvnw.cmd
~~~

Spring Initializr generates a Maven wrapper so the project can use its expected Maven version without requiring a global Maven install.

## Keep configuration outside Java code

~~~properties
server.port=8080
app.catalog.page-size=20
app.catalog.title=Learning shelf
~~~

A deployment can override properties with environment variables or another supported property source. Avoid committing passwords or API keys. Supply secrets through a protected environment or secret store.

For grouped settings, bind properties to a typed configuration class:

~~~java
@ConfigurationProperties(prefix = "app.catalog")
public record CatalogProperties(int pageSize, String title) {}
~~~

Enable scanning with @ConfigurationPropertiesScan on the application class. Typed configuration makes expected values discoverable and supports validation.

Profiles select environment-specific settings. A file named application-dev.properties is loaded when the dev profile is active. Profiles should describe configuration differences, not create separate copies of the application. Keep defaults safe and document which external settings each environment needs.

## Questions to review

1. Where does main Java source usually live?
2. Where are application properties stored?
3. What does the Maven wrapper provide?
4. How can deployment configuration override a default property?
5. Why should secrets stay out of committed configuration?
6. What does @ConfigurationProperties bind?
7. How can the dev profile be activated?
8. What should a profile represent?

| [Previous: Spring Boot foundations and first application](../chapters/01-foundations-and-first-application.md) | [Notes index](../README.md) | [Next: Beans and dependency injection](../chapters/03-beans-and-dependency-injection.md) |
|:--|:--:|--:|

