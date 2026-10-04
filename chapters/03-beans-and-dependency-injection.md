# 3. Beans and dependency injection

[Back to notes index](../README.md)

| [Previous: Project structure and configuration](../chapters/02-project-structure-and-configuration.md) | [Notes index](../README.md) | [Next: REST controllers and HTTP requests](../chapters/04-rest-controllers-and-http-requests.md) |
|:--|:--:|--:|

## What I am learning here

A Spring bean is an object managed by the application context. Dependency injection lets Spring provide an object's collaborators instead of having the object construct them directly. Constructor injection makes required dependencies visible and allows the class to be created in a plain unit test.

## Inject a required collaborator

~~~java
public interface NoticeSender {
    void send(String destination, String message);
}
~~~

~~~java
@Component
class ConsoleNoticeSender implements NoticeSender {
    @Override
    public void send(String destination, String message) {
        System.out.println("Sending to " + destination + ": " + message);
    }
}
~~~

~~~java
@Service
public class ReminderService {
    private final NoticeSender noticeSender;

    public ReminderService(NoticeSender noticeSender) {
        this.noticeSender = noticeSender;
    }

    public void remind(String email) {
        noticeSender.send(email, "Your reminder is ready.");
    }
}
~~~

Spring finds the component and passes it into ReminderService. A class with one constructor does not need @Autowired on that constructor. The final field shows that the dependency cannot be replaced after construction.

Use @Component for a general managed class and more specific stereotypes such as @Service or @Repository when they express the class role. Use @Bean inside a @Configuration class when creating an object from a third-party library that cannot be annotated directly.

Keep one clear implementation for each injected interface or qualify alternatives deliberately. Avoid field injection because it hides required dependencies and makes isolated testing harder.

## Questions to review

1. What is a Spring bean?
2. What does dependency injection do?
3. Why is constructor injection useful?
4. Why can a required dependency be final?
5. Is @Autowired required on a single constructor?
6. When is @Bean useful?
7. What does @Repository communicate?
8. What should I do when several beans implement one interface?

| [Previous: Project structure and configuration](../chapters/02-project-structure-and-configuration.md) | [Notes index](../README.md) | [Next: REST controllers and HTTP requests](../chapters/04-rest-controllers-and-http-requests.md) |
|:--|:--:|--:|

